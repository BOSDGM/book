# Gunicorn

独角兽. 一般用于管理多个进程, 有进程挂了可以拉起来, 防止服务器长时间停止服务, 还可以动态的调整worker数量.

Nginx主要负责静态文件, Gunicorn负责动态连接处理