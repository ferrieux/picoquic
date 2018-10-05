# picoquic

Minimalist implementation of the QUIC protocol, as defined by the IETF.
The IETF spec started with the version of QUIC defined by Google and
implemented in Chrome, but the IETF spec is independent of Chrome, and
does not attempt to be backward compatible. The main developer is 
Christian Huitema.

The first goal of this project is to provide feedback on the development
of a QUIC standard in the IETF QUIC WG. Information on the WG is available at
https://datatracker.ietf.org/wg/quic/charter/. The in-progress version of
the spec is available on GitHub at https://github.com/quicwg.

The second goal is to experiment with API for non-HTTP development, such as
DNS over QUIC. Then there are plenty of other features we may dream off,
such as support for multipath, or support for peer-to-peer applications.
That's on the horizon, but not there now.

The code in this repo is a work in progress. In fact, the specification itself
is a work in progress. The working group is progressing by running a series
of meetings and of interop trials between several implementations, listed
at https://github.com/quicwg/base-drafts/wiki/Implementations. The current
interoperability matrix is listed at
https://docs.google.com/spreadsheets/d/1D0tW89vOoaScs3IY9RGC0UesWGAwE6xyLk0l4JtvTVg/edit#gid=273618597.

Bastian Köcher has developed bindings of the picoquic library to RUST (https://www.rust-lang.org/en-US/). 
His repository can be found here: https://github.com/bkchr/picoquic-rs.
You may want to check it.


# Development

Picoquic is currently developed as a Visual Studio 2017 project,
and simultaneously tested on Windows and on Linux. It has a dependency
on the Picotls implementation of TLS 1.3 (https://github.com/h2o/picotls).
Picotls has two mode, a feature rich version that depends on OpenSSL, and a
leaner version that only depends on the "minicrypto" library. For now,
Picoquic uses the OpenSSL version, and has a dependency on OpenSSL.

The project consists of a core library (picoquic), of a test library
(picoquictest), and of a test program (picoquicfirst). All these are
written in C. In the Visual Studio project, the
test library is wrapped up in the Visual Studio unittest framework, which
makes for convenient regression testing during development. In the Linux
builds, the tests are run through a command line program.

# Milestones

As explained in the Wiki, Picoquic is actively tested against other implementations
during the QUIC Interop days. See https://github.com/private-octopus/picoquic/wiki/QUIC-milestones-and-interop-testing.

The current version is aligned with draft 15. Most big features are now tested, including
the revised interface between QUIC and TLS, but we still have to fully develop and 
test key rollover.

In parallel, we still plan to do an implementation
of DNS over QUIC (https://datatracker.ietf.org/doc/draft-huitema-quic-dnsoquic/).

We are spending time bettering the implementation. Until now 
the focus has been on correctness rather than performance. We will keep correctness,
but we will improve performance, especially in light of practical experience with 
applications. Suggestions are wellcome.

# Building Picoquic

Picoquic is developed in C, and can be built under Windows or Linux. Building the
project requires first managing the dependencies, Picotls (https://github.com/h2o/picotls)
and OpenSSL. Please note that you will need a recent version of Picotls --
the TLS API depends on commits from Aug 14, 2018, and the support for the
EOED removal extension of TLS 1.3 depends on commits from Oct 4, 2018.

## Picoquic on Windows

To build Picoquic on Windows, you need to:

 * Install and build Openssl on your machine

 * Document the location of the Openssl install in the environment variable OPENSSLDIR
   (OPENSSL64DIR for the x64 builds)

 * Clone and compile Picotls, using the Picotls for Windows options

 * Clone and compile Picoquic, using the Visual Studio 2017 solution picoquic.sln included in 
   the sources.

 * You can use the unit tests included in the Visual Studio solution to verify the port.

## Picoquic on Linux

The build experience on Linux is now much improved, thanks to check-ins from Deb Banerjee
and Igor Lubashev. 

To build Picoquic on Linux, you need to:

 * Install and build Openssl on your machine

 * Clone and compile Picotls, using cmake as explained in the Picotls documentation.

 * Clone and compile Picoquic:
~~~
   cmake .
   make
~~~
 * Run the test program "picoquic_ct" to verify the port.

## Picoquic on MacOSX

Thanks to Frederik Deweerdt for ensuring that Picoquic runs on MacOSX. The build steps
are the same as for Linux.

## Picoquic on FreeBSD

Same build steps as Linux. Picoquic probably also works on other BSD variants, but only FreeBSD
has been tested so far.

## Developing applications

Sorry, not all that much documentation yet. This will come as we populate the wiki. Your
best bet is to look at the demonstration program "picoquicdemo" that is included in the
release. The sources are in "picoquicfirst/picoquicdemo.c".

## Testing previous versions

The code is constantly updated to track the latest version of the specification. When we can,
we maintain compatibility with previous drafts, so as to facilitate interop testing with
other implementations. But some version changes will change the wire format or the encryption
code in incompatible ways. That's the reason why we only support draft-15 at this stage,
although both the client and server can be launched in draft-14 compatibility mode,
using the option "-v FF00000E".



 



