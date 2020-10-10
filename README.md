# Webtin

This is a fork of: https://github.com/rpolve/webtin

- Changed the Dockerfile to not use volume mounts so I could deployte it to google cloud.
- Pre-popped a config for a mud so it autostarts.
- Puts the client in child lock so the user can't switch to another mud. 
- Set Repeat Enter & Split on by default.


# How to Build

```
docker build -t fmtt:prod .
docker run -p 3000:3000 fmtt:prof &
```
