<p align="center">
  <img src="https://cdn.rawgit.com/awnumar/memguard/master/logo.svg" height="140" />
  <h3 align="center">MemGuard</h3>
  <p align="center">Secure software enclave for storage of sensitive information in memory.</p>
  <p align="center">
    <a href="https://travis-ci.org/awnumar/memguard"><img src="https://travis-ci.org/awnumar/memguard.svg?branch=master"></a>
    <a href="https://ci.appveyor.com/project/awnumar/memguard/branch/master"><img src="https://ci.appveyor.com/api/projects/status/nrtqmdolndm0pcac/branch/master?svg=true"></a>
    <a href="https://godoc.org/github.com/awnumar/memguard"><img src="https://godoc.org/github.com/awnumar/memguard?status.svg"></a>
    <a href="https://goreportcard.com/report/github.com/awnumar/memguard"><img src="https://goreportcard.com/badge/github.com/awnumar/memguard"></a>
  </p>
</p>

---

This package attempts to reduce the likelihood of sensitive data being exposed. It supports all major operating systems and is written in pure Go.

## Features

* Sensitive data is encrypted and authenticated in memory using xSalsa20 and Poly1305 respectively. The scheme also defends against cold-boot attacks.
* Memory allocation bypasses the language runtime by using system calls to query the kernel for resources directly. This avoids interference from the garbage-collector.
* Buffers that store plaintext data are fortified with guard pages and canary values to detect spurious accesses and overflows.
* Effort is taken to prevent sensitive data from touching the disk. This includes locking memory to prevent swapping and handling core dumps.
* Kernel-level immutability is implemented so that attempted modification of protected regions results in an access violation.
* Multiple endpoints provide session purging and safe termination capabilities as well as signal handling to prevent remnant data being left behind.
* Side-channel attacks are mitigated against by making sure that the copying and comparison of data is done in constant-time.
* Accidental memory leaks are mitigated against by harnessing the garbage-collector to automatically destroy containers that have become unreachable.

Some features were inspired by [libsodium](https://github.com/jedisct1/libsodium), so credits to them.

Full documentation and a complete overview of the API can be found [here](https://godoc.org/github.com/awnumar/memguard). Interesting and useful code samples can be found within the [examples](examples) subpackage.

## Installation

```
$ go get github.com/awnumar/memguard
```

We **strongly** encourage you to pin a specific version for a clean and reliable build. This can be accomplished using [modules](https://github.com/golang/go/wiki/Modules).

## Contributing

* Using the package and identifying points of friction.
* Reading the source code and looking for improvements.
* Adding interesting and useful program samples to [`./examples`](examples).
* Developing Proof-of-Concept attacks and mitigations.
* Improving compatibility with more kernels and architectures.
* Implementing kernel-specific and cpu-specific protections.
* Writing useful security and crypto libraries that utilise memguard.
* Submitting performance improvements or benchmarking code.

Issues are for reporting bugs and for discussion on proposals. Pull requests should be made against master.

## Future goals

* Ability to stream data to and from encrypted enclave objects.
* Catch segmentation faults to wipe memory before crashing.
* Evaluate and improve the strategies in place, particularly for [Coffer](core/coffer.go) objects.
* Formalise a threat model and evaluate our performance in regards to it.
* Use lessons learned to apply patches upstream to the Go language and runtime.
