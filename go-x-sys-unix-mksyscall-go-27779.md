## x/sys/unix: replace "mksyscall.pl" scripts with Go scripts [#27779](https://github.com/golang/go/issues/27779)

Repo: https://github.com/viswesr/sys/blob/master/unix/mksyscall.go 

mksyscall.pl and $GOROOT/src/syscall/mksyscall_windows.go use totally different approaches. The aim is to use the best of mksyscall_windows.go, merge all the various features of mksyscall.pl and produce no diff zsyscall*.go files

## LOG

### 18-Nov-2018

- Removed trace related tag and its implementaion
- Add command line and build tags printing in template
- Fix slice temp variable generation
- Partial support for more than 2 return parameters. Windows version expects at most two return parameters in which one being error. In unix version there could be upto 3 return parameters. This needs rewrite of various methods related to **Rets**
- Support non-blocking sycalls generation
- The overall time taken to execute *go run mksycall.go* is very slow compared to *mksyscall.pl*. Compiled mksyscall.go is around two times slower than mksyscall.pl

### 17-Nov-2018

* Sourced $GOROOT/src/syscall/mksyscall_windows.go and created mksyscall.go
* Added support for all available tags in mksyscall.pl as flags
* Added support for extraction of optional SYS_<NAME> parameter and detection of either //sys or //sysnb in ParseFiles()
* Various modifications to template and helper functions to produce syscall closer to mksyscall.pl 
* Need to implement usage of tags/flags
* Testing:  
  * ~/dev/sys/unix $ ./mksyscall.pl -tags linux,amd64 syscall_linux.go syscall_linux_amd64.go > gen_1.go
  * ~/dev/sys/unix $ go run mksyscall.go -tags linux,amd64 syscall_linux.go syscall_linux_amd64.go > gen_2.go
  * ~/dev/sys/unix $ diff -y gen_1.go gen_2.go  #or use your favourite source code compare tool
