---
layout: post
title:  "edx-docker"
date:   2016-05-30
categories: notes
---

使用官方提供的docker/build/edxapp 创建镜像,启动运行就失败.

```shell

/edx/app/supervisor/venvs/supervisor/bin/supervisord -n -c /edx/app/supervisor/supervisord.conf

```

lms,cms启动后又失败

```shell
2016-05-30 06:41:03,970 CRIT Supervisor running as root (no user in config file)
2016-05-30 06:41:03,971 WARN Included extra file "/edx/app/supervisor/conf.d/cms.conf" during parsing
2016-05-30 06:41:03,972 WARN Included extra file "/edx/app/supervisor/conf.d/edxapp.conf" during parsing
2016-05-30 06:41:03,974 WARN Included extra file "/edx/app/supervisor/conf.d/lms.conf" during parsing
2016-05-30 06:41:03,996 INFO RPC interface 'supervisor' initialized
2016-05-30 06:41:03,997 CRIT Server 'inet_http_server' running without any HTTP authentication checking
2016-05-30 06:41:03,998 INFO RPC interface 'supervisor' initialized
2016-05-30 06:41:03,999 CRIT Server 'unix_http_server' running without any HTTP authentication checking
2016-05-30 06:41:04,000 INFO supervisord started with pid 19
2016-05-30 06:41:05,005 INFO spawned: 'lms' with pid 22
2016-05-30 06:41:05,009 INFO spawned: 'cms' with pid 23
2016-05-30 06:41:06,013 INFO success: lms entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2016-05-30 06:41:06,014 INFO success: cms entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2016-05-30 06:41:10,872 INFO exited: cms (exit status 1; not expected)
2016-05-30 06:41:11,820 INFO spawned: 'cms' with pid 46
2016-05-30 06:41:12,146 INFO exited: lms (exit status 1; not expected)
```

检查supervisor

- supervisor配置文件 /edx/app/supervisor/supervisord.conf
- 相关日志位置:/edx/var/log/supervisor 

查看lms的日志输出
/edx/var/log/supervisor/lms-stderr.log

```shell

Traceback (most recent call last):
  File "/edx/app/edxapp/venvs/edxapp/bin/gunicorn", line 11, in <module>
    sys.exit(run())
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/app/wsgiapp.py", line 36, in run
    WSGIApplication("%(prog)s [OPTIONS] APP_MODULE").run()
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/app/base.py", line 135, in run
    Arbiter(self).run()
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/arbiter.py", line 59, in __init__
    self.setup(app)
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/arbiter.py", line 111, in setup
    self.app.wsgi()
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/app/base.py", line 106, in wsgi
    self.callable = self.load()
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/app/wsgiapp.py", line 27, in load
    return util.import_app(self.app_uri)
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/gunicorn/util.py", line 353, in import_app
    __import__(module)
  File "/edx/app/edxapp/edx-platform/lms/wsgi.py", line 26, in <module>
    startup.run()
  File "/edx/app/edxapp/edx-platform/lms/startup.py", line 50, in run
    django.setup()
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/django/__init__.py", line 17, in setup
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
  File "/edx/app/edxapp/venvs/edxapp/local/lib/python2.7/site-packages/django/utils/log.py", line 86, in configure_logging
    logging_config_func(logging_settings)
  File "/usr/lib/python2.7/logging/config.py", line 794, in dictConfig
    dictConfigClass(config).configure()
  File "/usr/lib/python2.7/logging/config.py", line 576, in configure
    '%r: %s' % (name, e))
ValueError: Unable to configure handler 'local': [Errno 2] No such file or directory
```