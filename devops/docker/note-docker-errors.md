# Docker Errors



## the input device is not a TTY. If you are using mintty, try prefixing the command with 'winpty'

Solution

Just add 'winpty' in start of your cmd ,Try below:
`$ winpty docker.exe run -it --rm ubuntu:14.04 /bin/bash`

References

- https://github.com/zeit/hyper/issues/2888

