[[meta title="__observe-file__ - do arbitrary command when observed files have changed"]]

# NAME

__observe-file__ - do arbitrary command when observed files have changed

# SYNOPSIS

__observe-file__ __-c__ _COMMAND_ ( _FILE_ | _DIR_ ) [ ( _FILE_ | _DIR_ ) ... ]

__observe-file__ __-x__ _FILE_

# OPTIONS

- __-c__ _COMMAND_

Do command _COMMAND_ when following files have changed.

You can also specify directory name to observe files in directory recursively.

- __-x__ _FILE_

observe _FILE_ and do _FILE_.

This is equivalent:

  observe-file -c I<FILE> I<FILE>

# EXAMPLES

## edit and run simple script

  observe-file -x ./foo.pl

  observe-file -c "perl -wcT ./foo.pl && ./foo.pl" ./foo.pl

## build and run automatically

  observe-file -c "make myprog && ./myprog"  myprog.c myprog.h libfoo.c

  observe-file -c "make myprog && ./myprog"  src/

## reload Firefox when contents have changed

  observe-file -c "reload-firefox localhost"  *.html *.css *.js

  observe-file -c "deploy && reload-firefox mymacbook"  webapp/

"reload-filefox" is simple shell script to request Firefox with MozRepl.

  #!/bin/sh
  [ $# -eq 1 -o $# -eq 2 ] || { echo 'usage: reload-firefox HOST PORT'; exit 1; }
  host=$1
  port=${2:-4242}
  

  echo "reload: $host:$port"
  cat <<EOF | nc $host $port
  content.location.reload(true)
  repl.quit()
  EOF
  echo

# REPOSITORY

<http://github.com/hirose31/observe-file>

# COPYRIGHT & LICENSE

This program is free software; you can redistribute it and/or modify it
under the same terms as Perl itself.