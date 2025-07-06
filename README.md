# linjector-rs
一种比ptrace更好的so注入工具，魔改了linjector-cli，隐藏了其maps文件特征，需将so文件拷贝到app的私有目录中(例如: /data/app/~~K9AspQJE-ugzmrCq_sHOGA==/com.example.frida_gum_test-0nw_wYKUVlLFXGJxOJSqNA==/lib/arm64/libfridagumtest.so)，然后在data/local/tmp目录中通过linjector进行注入

Android port of [linux_injector](https://github.com/namazso/linux_injector). Library injection using /proc/mem, without ptrace. Only aarch64 is supported.

To get an idea of how it works, you can read the [blog post](https://erfur.github.io/blog/dev/code-injection-without-ptrace).

## Usage

```
Inject code into a running process using /proc/mem

Usage: linjector-cli [OPTIONS] --file <FILE>

Options:
  -p, --pid <PID>
          pid of the target process

  -a, --app-package-name <APP_PACKAGE_NAME>
          target application's package name, (re)start the application and do injection

  -f, --file <FILE>
          path of the library/shellcode to inject

  -i, --injection-type <INJECTION_TYPE>
          type of injection
          
          [default: raw-dlopen]

          Possible values:
          - raw-dlopen:    Use dlopen to inject a library
          - memfd-dlopen:  Use memfd_create and dlopen to inject a library
          - raw-shellcode: Inject raw shellcode

      --func-sym <FUNC_SYM>
          function to hijack for injection, in the form "lib.so!symbol_name"

      --var-sym <VAR_SYM>
          variable to hijack for injection, in the form "lib.so!symbol_name"

  -d, --debug
          enable debug logs

      --logcat
          print logs to logcat

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```

## Modes

**Currently only raw dlopen mode works**. Since SELinux doesn't allow calling dlopen on a memfd, memfd dlopen will not work. Shellcode mode is not yet implemented.

