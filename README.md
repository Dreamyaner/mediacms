# MediaCMS

[![GitHub license](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://raw.githubusercontent.com/mediacms-io/mediacms/main/LICENSE.txt)
[![Releases](https://img.shields.io/github/v/release/mediacms-io/mediacms?color=green)](https://github.com/mediacms-io/mediacms/releases/)
[![DockerHub](https://img.shields.io/docker/pulls/mediacms/mediacms)](https://hub.docker.com/r/mediacms/mediacms)

MediaCMS 是一个现代化、功能齐全的开源视频和媒体内容管理系统。它旨在满足现代网络平台观看和分享媒体的需求。您可以在几分钟内构建一个小型到中型的视频和媒体门户。

它主要使用现代技术栈 Django + React 构建，并包含一个 REST API。

可以在 https://demo.mediacms.io 访问演示。

## 截图

<p align="center">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/index.jpg" width="340">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/video.jpg" width="340">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/embed.jpg" width="340">
</p>

## 功能
- **完全控制您的数据**：自行托管！
- **支持多种发布工作流**：公开、私密、未列出和自定义
- **现代技术**：Django/Python/Celery, React
- **支持多种媒体类型**：视频、音频、图片、PDF
- **多种媒体分类选项**：类别、标签和自定义
- **多种媒体分享选项**：社交媒体分享、视频嵌入代码生成
- **便捷的媒体搜索**：支持实时搜索功能
- **音视频内容的播放列表**：创建播放列表，添加和重新排序内容
- **响应式设计**：包括浅色和深色主题
- **高级用户管理**：允许自注册、仅限邀请、关闭注册
- **可配置的操作**：允许下载、添加评论、点赞、点踩、举报媒体
- **配置选项**：更改标志、字体、样式，添加更多页面
- **增强的视频播放器**：定制的视频.js 播放器，支持多种分辨率和播放速度选项
- **多种转码配置**：为多种尺寸（240p、360p、480p、720p、1080p）和多种配置（h264、h265、vp9）提供合理的默认设置
- **自适应视频流**：通过 HLS 协议实现
- **字幕/CC**：支持多语言字幕文件
- **可扩展的转码**：通过优先级进行转码。实验性支持远程工作者
- **分块文件上传**：支持可暂停/恢复的内容上传
- **REST API**：通过 Swagger 进行文档化
- **翻译**：大部分 CMS 已翻译成多种语言

## 示例案例

- **学校，教育。** 管理员和编辑决定发布哪些内容，学生不会被广告和无关内容分心，并且他们可以选择流媒体播放或下载内容。
- **组织敏感内容。** 在内容敏感且不能上传到外部网站的情况下。
- **建立一个伟大的社区。** MediaCMS 可以自定义（URL、标志、字体、美学），以便您为您的社区创建一个高度定制的视频门户！
- **个人门户。** 以您喜欢的方式组织、分类和托管您的内容。

## 哲学

我们相信需要高质量的开源网络应用程序来构建社区门户并支持协作。
我们对 MediaCMS 有三个目标：a) 提供现代系统所需的所有功能，b) 允许轻松安装和维护，c) 允许轻松定制和添加功能。

## 许可证

MediaCMS 根据 [GNU Affero 通用公共许可证 v3.0](LICENSE.txt) 发布。
版权所有 Markos Gogoulos。

## 支持和付费服务

我们提供定制安装、额外功能开发、从现有系统迁移、与遗留系统集成、培训和支持。更多信息请联系 info

### 商业托管
**Elestio**

您可以使用一键部署在 Elestio 上部署 MediaCMS。Elestio 通过提供收入分成来支持 MediaCMS，因此请点击下面的按钮来部署和使用 MediaCMS。

[![Deploy on Elestio](https://elest.io/images/logos/deploy-to-elestio-btn.png)](https://elest.io/open-source/mediacms)

## 硬件考虑

对于一个小型到中型的安装，每天上传几小时的视频，几百个活跃用户观看内容，最低配置为 4GB 内存 / 2-4 个 CPU。对于一个大型安装，每天上传许多小时的视频，建议增加更多的 CPU 和内存。

在磁盘空间方面，考虑需求。一般规则是将预期上传视频的大小乘以三（因为系统保留原始版本、编码版本和 HLS），所以如果每天接收 1G 的视频并保留所有视频，您应该考虑一年内使用 1T 的磁盘空间（1G * 3 * 365）。

## 安装 / 维护

有两种方式运行 MediaCMS，通过 Docker Compose 和通过自动化脚本在服务器上安装和配置所有需要的服务。请参阅相关页面：

- [单服务器](docs/admins_docs.md#2-server-installation) 页面
- [Docker Compose](docs/admins_docs.md#3-docker-installation) 页面

  完整指南请参阅博客文章 [如何在 2021 年自托管和分享您的视频](https://medium.com/@MediaCMS.io/how-to-self-host-and-share-your-videos-in-2021-14067e3b291b)。

## 配置

访问 [配置](docs/admins_docs.md#5-configuration) 页面。

## 开发者信息
请查看 [开发者体验](docs/dev_exp.md) 页面中的新部分。

## 文档

* [用户文档](docs/user_docs.md) 页面
* [管理员文档](docs/admins_docs.md) 页面
* [开发者文档](docs/developers_docs.md) 页面

## 技术

该软件使用以下优秀技术：Python, Django, Django Rest Framework, Celery, PostgreSQL, Redis, Nginx, uWSGI, React, Fine Uploader, video.js, FFMPEG, Bento4

## 谁在使用

- **Cinemata** 非营利媒体、技术和文化组织 - https://cinemata.org
- **Critical Commons** 公共媒体档案和合理使用倡导网络 - https://criticalcommons.org
- **美国妇科腹腔镜学会** - https://surgeryu.org/

## 如何贡献

如果您喜欢这个项目，您可以做以下几件事：
- 雇佣我们，进行定制安装、培训、支持、维护工作
- 向有兴趣雇佣我们的人推荐我们
- 写一篇关于 MediaCMS 的博客文章/文章
- 在社交媒体上分享这个项目
- 提出问题，参与 [讨论](https://github.com/mediacms-io/mediacms/discussions)，报告错误，提出建议
- [展示和分享](https://github.com/mediacms-io/mediacms/discussions/categories/show-and-tell) 您如何使用这个项目
- 给项目加星
- 添加功能，提交 PR，修复问题！

## 联系方式

info@mediacms.io


# MediaCMS

[![GitHub license](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://raw.githubusercontent.com/mediacms-io/mediacms/main/LICENSE.txt)
[![Releases](https://img.shields.io/github/v/release/mediacms-io/mediacms?color=green)](https://github.com/mediacms-io/mediacms/releases/)
[![DockerHub](https://img.shields.io/docker/pulls/mediacms/mediacms)](https://hub.docker.com/r/mediacms/mediacms)



MediaCMS is a modern, fully featured open source video and media CMS. It is developed to meet the needs of modern web platforms for viewing and sharing media. It can be used to build a small to medium video and media portal within minutes.

It is built mostly using the modern stack Django + React and includes a REST API.

A demo is available at https://demo.mediacms.io


## Screenshots

<p align="center">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/index.jpg" width="340">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/video.jpg" width="340">
    <img src="https://raw.githubusercontent.com/mediacms-io/mediacms/main/docs/images/embed.jpg" width="340">
</p>

## Features
- **Complete control over your data**: host it yourself!
- **Support for multiple publishing workflows**: public, private, unlisted and custom
- **Modern technologies**: Django/Python/Celery, React.
- **Multiple media types support**: video, audio,  image, pdf
- **Multiple media classification options**: categories, tags and custom
- **Multiple media sharing options**: social media share, videos embed code generation
- **Easy media searching**: enriched with live search functionality
- **Playlists for audio and video content**: create playlists, add and reorder content
- **Responsive design**: including light and dark themes
- **Advanced users management**: allow self registration, invite only, closed.
- **Configurable actions**: allow download, add comments, add likes, dislikes, report media
- **Configuration options**: change logos, fonts, styling, add more pages
- **Enhanced video player**: customized video.js player with multiple resolution and playback speed options
- **Multiple transcoding profiles**: sane defaults for multiple dimensions (240p, 360p, 480p, 720p, 1080p) and multiple profiles (h264, h265, vp9)
- **Adaptive video streaming**: possible through HLS protocol
- **Subtitles/CC**: support for multilingual subtitle files
- **Scalable transcoding**: transcoding through priorities. Experimental support for remote workers
- **Chunked file uploads**: for pausable/resumable upload of content
- **REST API**: Documented through Swagger
- **Translation**: Most of the CMS is translated to a number of languages

## Example cases

- **Schools, education.** Administrators and editors keep what content will be published, students are not distracted with advertisements and irrelevant content, plus they have the ability to select either to stream or download content.
- **Organization sensitive content.** In cases where content is sensitive and cannot be uploaded to external sites.
- **Build a great community.** MediaCMS can be customized (URLs, logos, fonts, aesthetics) so that you create a highly customized video portal for your community!
- **Personal portal.** Organize, categorize and host your content the way you prefer.


## Philosophy

We believe there's a need for quality open source web applications that can be used to build community portals and support collaboration.
We have three goals for MediaCMS: a) deliver all functionality one would expect from a modern system, b) allow for easy installation and maintenance, c) allow easy customization and addition of features.


## License

MediaCMS is released under [GNU Affero General Public License v3.0 license](LICENSE.txt).
Copyright Markos Gogoulos.


## Support and paid services

We provide custom installations, development of extra functionality, migration from existing systems, integrations with legacy systems, training and support. Contact us at info@mediacms.io for more information.

### Commercial Hostings
**Elestio**

You can deploy MediaCMS on Elestio using one-click deployment. Elestio supports MediaCMS by providing revenue share so go ahead and click below to deploy and use MediaCMS.

[![Deploy on Elestio](https://elest.io/images/logos/deploy-to-elestio-btn.png)](https://elest.io/open-source/mediacms)

## Hardware considerations

For a small to medium installation, with a few hours of video uploaded daily, and a few hundreds of active daily users viewing content, 4GB Ram / 2-4 CPUs as minimum is ok. For a larger installation with many hours of video uploaded daily, consider adding more CPUs and more Ram.

In terms of disk space, think of what the needs will be. A general rule is to multiply by three the size of the expected uploaded videos (since the system keeps original versions, encoded versions plus HLS), so if you receive 1G of videos daily and maintain all of them, you should consider a 1T disk across a year (1G * 3 * 365).


## Installation / Maintanance

There are two ways to run MediaCMS, through Docker Compose and through installing it on a server via an automation script that installs and configures all needed services. Find the related pages:

- [Single Server](docs/admins_docs.md#2-server-installation) page
- [Docker Compose](docs/admins_docs.md#3-docker-installation) page

  A complete guide can be found on the blog post [How to self-host and share your videos in 2021](https://medium.com/@MediaCMS.io/how-to-self-host-and-share-your-videos-in-2021-14067e3b291b).

## Configuration

Visit [Configuration](docs/admins_docs.md#5-configuration) page.


## Information for developers
Check out the new section on the [Developer Experience](docs/dev_exp.md) page


## Documentation

* [Users documentation](docs/user_docs.md) page
* [Administrators documentation](docs/admins_docs.md) page
* [Developers documentation](docs/developers_docs.md) page


## Technology

This software uses the following list of awesome technologies: Python, Django, Django Rest Framework, Celery, PostgreSQL, Redis, Nginx, uWSGI, React, Fine Uploader, video.js, FFMPEG, Bento4


## Who is using it

- **Cinemata** non-profit media, technology and culture organization - https://cinemata.org
- **Critical Commons** public media archive and fair use advocacy network - https://criticalcommons.org
- **American Association of Gynecologic Laparoscopists** - https://surgeryu.org/


## How to contribute

If you like the project, here's a few things you can do
- Hire us, for custom installations, training, support, maintenance work
- Suggest us to others that are interested to hire us
- Write a blog post/article about MediaCMS
- Share on social media about the project
- Open issues, participate on [discussions](https://github.com/mediacms-io/mediacms/discussions), report bugs, suggest ideas
- [Show and tell](https://github.com/mediacms-io/mediacms/discussions/categories/show-and-tell) how you are using the project
- Star the project
- Add functionality, work on a PR, fix an issue!


## Contact

info@mediacms.io