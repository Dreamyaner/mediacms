# 开发者文档

## 目录
- [1. 欢迎](#1-welcome)
- [2. 系统架构](#2-system-architecture)
- [3. API 文档](#3-api-documentation)
- [4. 如何贡献](#4-how-to-contribute)
- [5. 使用 Docker 的提示](#5-working-with-docker-tips)
- [6. 使用自动化测试](#6-working-with-the-automated-tests)
- [7. 视频转码原理](#7-how-video-is-transcoded)

## 1. 欢迎
此页面是为 MediaCMS 开发者创建的，包含相关信息。

## 2. 系统架构
待编写

## 3. API 文档
API 使用 Swagger 进行文档化 - 请访问 http://your_installation/swagger - 示例 https://demo.mediacms.io/swagger/
此页面允许您登录以执行认证操作 - 如果已登录，它还将使用您的会话。

使用 Python requests 库的示例：

```python
import requests

auth = ('user', 'password')
upload_url = "https://domain/api/v1/media"
title = 'x title'
description = 'x description'
media_file = '/tmp/file.mp4'

requests.post(
    url=upload_url,
    files={'media_file': open(media_file, 'rb')},
    data={'title': title, 'description': description},
    auth=auth
)
```

## 4. 如何贡献
在发送 PR 之前，请确保您的代码格式正确。为此，请使用 `pre-commit install` 安装 pre-commit 钩子，并运行 `pre-commit run --all` 以修复所有问题，然后再提交代码。每次提交代码时，此 pre-commit 钩子将检查您的代码格式。

如果您想为此仓库做贡献，请查看 [行为准则页面](../CODE_OF_CONDUCT.md)。

## 5. 使用 Docker 的提示

要执行 Docker 安装，请按照说明安装 Docker 和 Docker Compose（docs/Docker_Compose.md），然后构建/启动 docker-compose-dev.yaml。这将在所有其他容器（包括端口 80 上的 Django Web 应用程序）之上在端口 8088 上运行前端应用程序。

```
docker-compose -f docker-compose-dev.yaml build
docker-compose -f docker-compose-dev.yaml up
```

在安装过程中会创建一个 `admin` 用户。其属性在 `docker-compose-dev.yaml` 中定义：
```
ADMIN_USER: 'admin'
ADMIN_PASSWORD: 'admin'
ADMIN_EMAIL: 'admin@localhost'
```


### 前端应用程序更改
例如更改 `frontend/src/static/js/pages/HomePage.tsx`，开发应用程序会在几秒钟内刷新（热重载），我看到更改后，如果满意，可以运行

```
docker-compose -f docker-compose-dev.yaml exec -T frontend npm run dist
```
然后为了使更改在通过 nginx 提供的应用程序中可见，

```
cp -r frontend/dist/static/* static/
```

POST 调用：不能通过开发服务器执行，必须通过正常应用程序（端口 80）进行，然后在端口 8088 上的开发应用程序中查看更改。确保在 `frontend/.env` 中设置了 URL（如果不同于 localhost）。

媒体页面：需要通过主应用程序（nginx/端口 80）上传内容，然后使用页面 media.html 的 ID，例如 `http://localhost:8088/media.html?m=nc9rotyWP`还有一些 CORS 问题需要解决，以便某些页面正常运行，例如管理评论页面

```
http://localhost:8088/manage-media.html manage_media
```

### 后端应用程序更改
在对 Django 应用程序进行更改后（例如更改 `files/forms.py`），为了查看更改，我必须重新启动 Web 容器

```
docker-compose -f docker-compose-dev.yaml restart web
```


## 视频如何转码

原始文件上传到应用程序服务器，并作为 FileFields 存储在那里。

如果文件是视频且持续时间超过一定时间（在设置中定义，我认为是 4 分钟），它们也会被分成块，因此每个块都有一个 Encode 对象，适用于所有启用的 EncodeProfiles。

然后工人开始选择 Encode 对象并转码这些块，如果一个块成功转码，原始文件（小块）将被转码文件替换，并且 Encode 对象状态标记为“成功”。

original.mp4 (1G, 720px)--> Encode1 (100MB, 240px, chunk=True), Encode2 (100MB, 240px, chunk=True)...EncodeXX (100MB, 720px, chunk=True) ---> 当所有 Encode 对象对于某个分辨率都成功时，它们会被连接成 original_resolution.mp4 文件，并作为 Encode 对象存储（chunk=False）。这就是可供下载的内容。

显然，Encode 对象用于存储最终提供的转码文件（chunk=False，状态='success'），但也用于存储正在转码的文件（chunk=True，状态='pending/etc'）。

（括号内）
还有一个实验性的小服务（目前未提交到仓库），仅通过 API 进行通信，a）获取任务运行，b）返回结果。它发出请求并接收一个 ffmpeg 命令和一个文件，运行 ffmpeg 命令，并返回结果。我在多个安装中使用了这种机制，通过更多的服务器/CPU 迁移现有视频，只遇到一个问题，需要定期从服务器中删除一些临时文件（通过定期任务，不是很大的问题）。
（括号结束）

当 Encode 对象标记为成功且 chunk=False，并因此可供下载/流式传输时，会启动一个任务并保存文件的 HLS 版本（1 mp4-->x 个小 .ts 块）。这将是 FILES_C。

这种机制允许具有相同文件系统访问权限的工人（无论是本地主机，还是通过共享网络文件系统，例如 NFS/EFS）同时工作并生成结果。

## 6. 使用自动化测试

这些说明假设您正在使用 Docker 安装

1. 启动 docker-compose

```
docker-compose up
```

2. 在 web 容器上安装 `requirements-dev.txt` 中的依赖项（我们将使用 web 容器进行此操作）


```
docker-compose exec -T web pip install -r requirements-dev.txt 
```

3. 现在您可以运行现有的测试

```
docker-compose exec --env TESTING=True -T web pytest
```

传递 `TESTING=True` 是为了让 Django 知道这是一个测试环境（例如，它会将 Celery 任务作为函数运行，而不是作为后台任务运行，因为在 pytest 的情况下不会启动 Celery）

4. 您可以尝试单个测试，通过指定路径，例如

```
docker-compose exec --env TESTING=True -T web pytest tests/test_fixtures.py
```

5. 您还可以查看覆盖率

```
docker-compose exec --env TESTING=True -T web pytest --cov=. --cov-report=html
```

当然...我们非常欢迎您帮助我们提高覆盖率 ;)

# Developers documentation

## Table of contents
- [1. Welcome](#1-welcome)
- [2. System architecture](#2-system-architecture)
- [3. API documentation](#3-api-documentation)
- [4. How to contribute](#4-how-to-contribute)
- [5. Working with Docker tips](#5-working-with-docker-tips)
- [6. Working with the automated tests](#6-working-with-the-automated-tests)
- [7. How video is transcoded](#7-how-video-is-transcoded)

## 1. Welcome
This page is created for MediaCMS developers and contains related information.

## 2. System architecture
to be written

## 3. API documentation
API is documented using Swagger - checkout ot http://your_installation/swagger - example https://demo.mediacms.io/swagger/
This page allows you to login to perform authenticated actions - it will also use your session if logged in. 


An example of working with Python requests library:

```
import requests

auth = ('user' ,'password')
upload_url = "https://domain/api/v1/media"
title = 'x title'
description = 'x description'
media_file = '/tmp/file.mp4'

requests.post(
    url=upload_url,
    files={'media_file': open(media_file,'rb')},
    data={'title': title, 'description': description},
    auth=auth
)
```

## 4. How to contribute
Before you send a PR, make sure your code is properly formatted. For that, use `pre-commit install` to install a pre-commit hook and run `pre-commit run --all` and fix everything before you commit. This pre-commit will check for your code lint everytime you commit a code.

Checkout the [Code of conduct page](../CODE_OF_CONDUCT.md) if you want to contribute to this repository


## 5. Working with Docker tips

To perform the Docker installation, follow instructions to install Docker + Docker compose (docs/Docker_Compose.md) and then build/start docker-compose-dev.yaml . This will run the frontend application on port 8088 on top of all other containers (including the Django web application on port 80)

```
docker-compose -f docker-compose-dev.yaml build
docker-compose -f docker-compose-dev.yaml up
```

An `admin` user is created during the installation process. Its attributes are defined in `docker-compose-dev.yaml`:
```
ADMIN_USER: 'admin'
ADMIN_PASSWORD: 'admin'
ADMIN_EMAIL: 'admin@localhost'
```

### Frontend application changes
Eg change `frontend/src/static/js/pages/HomePage.tsx` , dev application refreshes in a number of seconds (hot reloading) and I see the changes, once I'm happy I can run

```
docker-compose -f docker-compose-dev.yaml exec -T frontend npm run dist
```

And then in order for the changes to be visible on the application while served through nginx, 

```
cp -r frontend/dist/static/* static/
```

POST calls: cannot be performed through the dev server, you have to make through the normal application (port 80) and then see changes on the dev application on port 8088. 
Make sure the urls are set on `frontend/.env` if different than localhost


Media page: need to upload content through the main application (nginx/port 80), and then use an id for page media.html, for example `http://localhost:8088/media.html?m=nc9rotyWP`

There are some issues with CORS too to resolve, in order for some pages to function, eg the manage comments page

```
http://localhost:8088/manage-media.html manage_media
```

### Backend application changes
After I make changes to the django application (eg make a change on `files/forms.py`) in order to see the changes I have to restart the web container

```
docker-compose -f docker-compose-dev.yaml restart web
```

## How video is transcoded

Original files get uploaded to the application server, and they get stored there as FileFields.

If files are videos and the duration is greater than a number (defined on settings, I think 4minutes), they are also broken in chunks, so one Encode object per chunk, for all enabled EncodeProfiles.

Then the workers start picking Encode objects and they transcode the chunks, so if a chunk gets transcoded correctly, the original file (the small chunk) gets replaced by the transcoded file, and the Encode object status is marked as 'success'.


original.mp4 (1G, 720px)--> Encode1 (100MB, 240px, chunk=True), Encode2 (100MB, 240px, chunk=True)...EncodeXX (100MB, 720px, chunk=True) ---> when all Encode objects are success, for a resolution, they get concatenated to the original_resolution.mp4 file and this gets stored as Encode object (chunk=False). This is what is available for download.

Apparently the Encode object is used to store Encoded files that are served eventually (chunk=False, status='success'), but also files while they are on their way to get transcoded (chunk=True, status='pending/etc')

(Parenthesis opening)
there is also an experimental small service (not commited to the repo currently) that speaks only through API and a) gets tasks to run, b) returns results. So it makes a request and receives an ffmpeg command, plus a file, it runs the ffmpeg command, and returns the result.I've used this mechanism on a number of installations to migrate existing videos through more servers/cpu and has worked with only one problem, some temporary files needed to be removed from the servers (through a periodic task, not so big problem)
(Parenthesis closing)


When the Encode object is marked as success and chunk=False, and thus is available for download/stream, there is a task that gets started and saves an HLS version of the file (1 mp4-->x number of small .ts chunks). This would be FILES_C

This mechanism allows for workers that have access on the same filesystem (either localhost, or through a shared network filesystem, eg NFS/EFS) to work on the same time and produce results. 

## 6. Working with the automated tests

This instructions assume that you're using the docker installation

1. start docker-compose

```
docker-compose up
```

2. Install the requirements on `requirements-dev.txt ` on web container (we'll use the web container for this)

```
docker-compose exec -T web pip install -r requirements-dev.txt 
```

3. Now you can run the existing tests

```
docker-compose exec --env TESTING=True -T web pytest
```

The `TESTING=True` is passed for Django to be aware this is a testing environment (so that it runs Celery tasks as functions for example and not as background tasks, since Celery is not started in the case of pytest)


4. You may try a single test, by specifying the path, for example

```
docker-compose exec --env TESTING=True -T web pytest tests/test_fixtures.py
```

5. You can also see the coverage

```
docker-compose exec --env TESTING=True -T web pytest --cov=. --cov-report=html
```

and of course...you are very welcome to help us increase it ;)
