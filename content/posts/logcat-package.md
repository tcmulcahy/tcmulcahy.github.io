+++
title = 'logcat, but just one package'
tags = ['android']
date = 2023-09-17T08:46:11-07:00
draft = false
+++
I like to view the logcat output for just my app and nothing else. Here's a
small shell script I use for this.
```
function logcat {
  pkg="$1"
  shift
  if [ -z "$pkg" ]; then
    >&2 echo 'Usage: logcat pkg ...'
    return 1
  fi

  uid="$(adb shell pm list package -U $pkg | sed 's/.*uid://')"
  if [ -z "$uid" ]; then
    >&2 echo "pkg '$pkg' not found"
    return 1
  fi

  adb logcat --uid="$uid" "$@"
}
```

Usage is:
```
# replace com.example with your app's package name
$ logcat com.example
```
