## Description
Adds the wait3, wait4, waitid, pause, sigsend, and getrusage methods to the Process module.

## Installation
`gem install proc-wait3`

## Synopsis
```ruby
require 'proc/wait3'

pid = fork{
  sleep 1
  exit 2
}

puts Time.now.to_s
Process.wait3
puts $?.exitstatus # => 2
```

## Tested Platforms
* Solaris
* Linux
* FreeBSD
* OS X

## Warnings
Linux users who compile with gcc -Wall will notice a few warnings. These
are harmless (and unavoidable atm).

Linux users may also notice warnings about implicit declarations. These
are also harmless, and can be silenced by installing the libbsd-dev package
first.

On Darwin these methods will likely fail with Errno::EINTR unless you pass
the WNOHANG flag. I am not sure why, I can only speculate that the child is
receiving a signal from either the Ruby interpreter or the operating system.

## Integration with Ruby's process.c
I considered simply providing a patch to the core process.c file, but I
decided against it for two reasons.  First, I wanted to get something
out more quickly rather than waiting for approval from the core developers
who, based on an earlier post, seem somewhat gun-shy about integrating support
for wait3() and wait4() based, I think, on portability concerns.

Second, and more importantly, I don't like the cProcStatus class.  The
extra inspection code seems like an awful lot of work for very little gain.
The overloaded methods are also overkill, and do nothing but save me the
trouble of typing the word "status", since all they're for is comparing or
operating on the status attribute.

## Additional Documentation
Please see the doc/wait3.txt file for detailed documentation.
