# PHP INI Configuration Before Deploy

## Overview

Essential `php.ini` settings to configure before deploying your PHP application to production.

## Why Configure php.ini Before Deployment?

- **Security**: Prevent information disclosure and reduce attack surface
- **Performance**: Optimize resource usage and response times
- **Stability**: Set appropriate limits to prevent crashes
- **Debugging**: Control error reporting for different environments


## Security Settings

### Hide PHP Version
```ini
expose_php = Off
```
**Why**: Prevents attackers from knowing your PHP version and targeting specific vulnerabilities.

### Disable Dangerous Functions
```ini
disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source
```
**Why**: These functions can execute system commands, which attackers can exploit if they find a vulnerability in your code.

### Disable Remote File Access
```ini
allow_url_fopen = Off
allow_url_include = Off
```
**Why**: Prevents attackers from including malicious remote files in your application.

### Error Display
```ini
display_errors = Off
display_startup_errors = Off
log_errors = On
error_log = /var/log/php/error.log
```
**Why**: Displaying errors reveals sensitive information about your application structure. Log errors instead for debugging.

## Performance Settings

### Memory Limit
```ini
memory_limit = 256M
```
**Why**: Controls how much memory each script can use. Prevents a single script from consuming all server resources.

### Execution Time
```ini
max_execution_time = 30
max_input_time = 60
```
**Why**: Limits how long scripts can run. Prevents scripts from running indefinitely and blocking server resources.

### File Upload Limits
```ini
upload_max_filesize = 20M
post_max_size = 25M
max_file_uploads = 20
```
**Why**: Controls upload sizes. `post_max_size` must be larger than `upload_max_filesize` because POST data includes file + form data.

### OPcache
```ini
opcache.enable = 1
opcache.memory_consumption = 128
opcache.interned_strings_buffer = 8
opcache.max_accelerated_files = 10000
opcache.revalidate_freq = 2
```
**Why**: Caches compiled PHP code in memory. Dramatically improves performance by avoiding repeated compilation.

## Session Security

### Secure Cookie Settings
```ini
session.cookie_httponly = 1
session.cookie_secure = 1
session.use_strict_mode = 1
session.cookie_samesite = "Lax"
```
**Why**: 
- `httponly`: Prevents JavaScript from accessing session cookies (XSS protection)
- `secure`: Only sends cookies over HTTPS
- `strict_mode`: Prevents session fixation attacks
- `samesite`: Protects against CSRF attacks

### Session Lifetime
```ini
session.gc_maxlifetime = 1440
```
**Why**: Sets how long inactive sessions remain valid (in seconds). 1440 = 24 minutes.

## Resource Limits

### Input Variables
```ini
max_input_vars = 1000
max_input_nesting_level = 64
```
**Why**: Limits the number of input variables and nesting depth. Prevents DoS attacks through excessive form data.

## Date/Time

### Timezone
```ini
date.timezone = "UTC"
```
**Why**: Sets default timezone for all date/time functions. Prevents inconsistent timestamps across your application.

## Quick Reference Template

```ini
; Security
expose_php = Off
allow_url_fopen = Off
allow_url_include = Off
disable_functions = exec,passthru,shell_exec,system,proc_open,popen
display_errors = Off
log_errors = On
error_log = /var/log/php/error.log

; Performance
memory_limit = 256M
max_execution_time = 30
max_input_time = 60
upload_max_filesize = 20M
post_max_size = 25M

; OPcache
opcache.enable = 1
opcache.memory_consumption = 128
opcache.max_accelerated_files = 10000

; Session Security
session.cookie_httponly = 1
session.cookie_secure = 1
session.use_strict_mode = 1
session.cookie_samesite = "Lax"

; Other
date.timezone = "UTC"
max_input_vars = 1000
```