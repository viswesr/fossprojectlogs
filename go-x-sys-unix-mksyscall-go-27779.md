## x/sys/unix: replace "mksyscall.pl" scripts with Go scripts [#27779](https://github.com/golang/go/issues/27779)

Repo: https://github.com/viswesr/sys/blob/master/unix/mksyscall.go 

mksyscall.pl and $GOROOT/src/syscall/mksyscall_windows.go use totally different approaches. The aim is to use the best of mksyscall_windows.go, merge all the various features of mksyscall.pl and produce no diff zsyscall*.go files

### 17-Nov-2018

* Sourced $GOROOT/src/syscall/mksyscall_windows.go and created mksyscall.go
* Added support for all available tags in mksyscall.pl as flags
* Added support for extraction of optional SYS_<NAME> parameter and detection of either //sys or //sysnb in ParseFiles()
* Various modifications to template and helper functions to produce syscall closer to mksyscaall.pl 
* Need to implement usage of tags/flags
* Testing:  
  * ~/dev/sys/unix $ ./mksyscall.pl -tags linux,amd64 syscall_linux.go syscall_linux_amd64.go > gen_1.go
  * ~/dev/sys/unix $ go run mksyscall.go -tags linux,amd64 syscall_linux.go syscall_linux_amd64.go > gen_2.go
  * ~/dev/sys/unix $ diff -y gen_1.go gen_2.go  #or use your favourite source code compare tool
