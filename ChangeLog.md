* [0.2.1](https://github.com/mwiede/jsch/releases/tag/jsch-0.2.1)
  * Allow to set a Logger per JSch-instance rather than a VM-wide one [#128](https://github.com/mwiede/jsch/pull/128)
  * Preliminary changes prior to Javadoc work [#126](https://github.com/mwiede/jsch/pull/126)
  * Remove check to allow setting any filename encoding with any server version [#137](https://github.com/mwiede/jsch/issues/137) [#142](https://github.com/mwiede/jsch/pull/142)
* [0.2.0](https://github.com/mwiede/jsch/releases/tag/jsch-0.2.0)
  * Disable RSA/SHA1 signature algorithm by default [#75](https://github.com/mwiede/jsch/issues/75)
  * Add basic Logger implementations that can be optionally utilized with `JSch.setLogger()`:
    * JulLogger, using `java.util.logging.Logger`
    * JplLogger, using [Java 9's JEP 264](https://openjdk.java.net/jeps/264)
    * Log4j2Logger, using [Apache Log4j 2](https://logging.apache.org/log4j/2.x/)
    * Slf4jLogger, using [SLF4J](https://www.slf4j.org/)
  * Fix client version to be compliant with [RFC 4253 section 4.2](https://datatracker.ietf.org/doc/html/rfc4253#section-4.2) by not including minus sign characters [#115](https://github.com/mwiede/jsch/issues/115)
  * Add `java.util.zip` based compression implementation [#114](https://github.com/mwiede/jsch/issues/114)
    * This is based upon the [CompressionJUZ implementation](http://www.jcraft.com/jsch/examples/CompressionJUZ.java) posted to the [JSch-users mailing list](https://sourceforge.net/p/jsch/mailman/jsch-users/thread/201202031343.WAA19979%40jcraft.com/#msg28781313) in 2012 by the original JSch author
    * The existing JZlib implementation remains the default to maintain strict [RFC 4253 section 6.2](https://datatracker.ietf.org/doc/html/rfc4253#section-6.2) compliance
      * To use the new implementation globally, execute `JSch.setConfig("zlib@openssh.com", "com.jcraft.jsch.juz.Compression")` + `JSch.setConfig("zlib", "com.jcraft.jsch.juz.Compression")`
      * To use the new implementation per session, execute `session.setConfig("zlib@openssh.com", "com.jcraft.jsch.juz.Compression")` + `session.setConfig("zlib", "com.jcraft.jsch.juz.Compression")`
* [0.1.72](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.72)
  * Switch chacha20-poly1305@<!-- -->openssh.com algorithm to a pure [Bouncy Castle](https://www.bouncycastle.org/java.html) based implementation
  * implement openssh config behavior to handle append, prepend and removal of algorithms [#104](https://github.com/mwiede/jsch/pull/104) 
* [0.1.71](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.71)
  * Address [#98](https://github.com/mwiede/jsch/issues/98) by restoring JSch.VERSION
* [0.1.70](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.70)
  * Address [#89](https://github.com/mwiede/jsch/issues/89) by fixing rare ECDSA signature validation issue
  * Address [#93](https://github.com/mwiede/jsch/issues/93) by always setting the "want reply" flag for "env" type channel requests to false
* [0.1.69](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.69)
  * Address [#83](https://github.com/mwiede/jsch/issues/83) by sending CR LF at the end of the identification string
  * Fix earlier change for [#76](https://github.com/mwiede/jsch/issues/76) that failed to correctly make the "Host" keyword case-insensitive
  * Fix PageantConnector struct class visibility [#86](https://github.com/mwiede/jsch/pull/86)
* [0.1.68](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.68)
  * Added support for the rijndael-cbc@<!-- -->lysator.liu.se algorithm
  * Added support for the hmac-ripemd160, hmac-ripemd160@<!-- -->openssh.com and hmac-ripemd160-etm@<!-- -->openssh.com algorithms using [Bouncy Castle](https://www.bouncycastle.org/java.html)
  * Added support for various algorithms from [RFC 4253](https://datatracker.ietf.org/doc/html/rfc4253) and [RFC 4344](https://datatracker.ietf.org/doc/html/rfc4344) using [Bouncy Castle](https://www.bouncycastle.org/java.html)
    * cast128-cbc
    * cast128-ctr
    * twofish-cbc
    * twofish128-cbc
    * twofish128-ctr
    * twofish192-cbc
    * twofish192-ctr
    * twofish256-cbc
    * twofish256-ctr
  * Added support for the seed-cbc@<!-- -->ssh.com algorithm using [Bouncy Castle](https://www.bouncycastle.org/java.html)
  * Address [#76](https://github.com/mwiede/jsch/issues/76) by making the "Host" keyword case-insensitive
* [0.1.67](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.67)
  * Added support for the blowfish-ctr algorithm from [RFC 4344](https://datatracker.ietf.org/doc/html/rfc4344)
  * Fix bug where ext-info-c was incorrectly advertised during rekeying
    * According to [RFC 8308 section 2.1](https://datatracker.ietf.org/doc/html/rfc8308#section-2.1), ext-info-c should only advertised during the first key exchange
  * Address [#77](https://github.com/mwiede/jsch/issues/77) by attempting to add compatibility with older [Bouncy Castle](https://www.bouncycastle.org/java.html) releases
* [0.1.66](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.66)
  * Added support for [RFC 8308](https://datatracker.ietf.org/doc/html/rfc8308) extension negotiation and server-sig-algs extension
    * This support is enabled by default, but can be controlled via the enable_server_sig_algs config option (or `jsch.enable_server_sig_algs` system property)
    * When enabled and a server-sig-algs message is received from the server, the algorithms included by the server and also present in the PubkeyAcceptedKeyTypes config option will be attempted first when using publickey authentication
    * Additionally if the server is detected as OpenSSH version 7.4, the rsa-sha2-256 & rsa-sha2-512 algorithms will be added to the received server-sig-algs as a workaround for [OpenSSH bug 2680](https://bugzilla.mindrot.org/show_bug.cgi?id=2680)
  * Added support for various algorithms supported by Tectia (ssh.com):
    * diffie-hellman-group14-sha224@<!-- -->ssh.com
    * diffie-hellman-group14-sha256@<!-- -->ssh.com
    * diffie-hellman-group15-sha256@<!-- -->ssh.com
    * diffie-hellman-group15-sha384@<!-- -->ssh.com
    * diffie-hellman-group16-sha384@<!-- -->ssh.com
    * diffie-hellman-group16-sha512@<!-- -->ssh.com
    * diffie-hellman-group18-sha512@<!-- -->ssh.com
    * diffie-hellman-group-exchange-sha224@<!-- -->ssh.com
    * diffie-hellman-group-exchange-sha384@<!-- -->ssh.com
    * diffie-hellman-group-exchange-sha512@<!-- -->ssh.com
    * hmac-sha224@<!-- -->ssh.com
    * hmac-sha256@<!-- -->ssh.com
    * hmac-sha256-2@<!-- -->ssh.com
    * hmac-sha384@<!-- -->ssh.com
    * hmac-sha512@<!-- -->ssh.com
    * ssh-rsa-sha224@<!-- -->ssh.com
    * ssh-rsa-sha256@<!-- -->ssh.com
    * ssh-rsa-sha384@<!-- -->ssh.com
    * ssh-rsa-sha512@<!-- -->ssh.com
  * Added support for SHA224 to FingerprintHash
  * Fixing [#52](https://github.com/mwiede/jsch/issues/52)
  * Deprecate `void setFilenameEncoding(String encoding)` in favor of `void setFilenameEncoding(Charset encoding)` in `ChannelSftp`
  * Added support for rsa-sha2-256 & rsa-rsa2-512 algorithms to `ChannelAgentForwarding`
  * Address [#65](https://github.com/mwiede/jsch/issues/65) by adding ssh-agent support derived from [jsch-agent-proxy](https://github.com/ymnk/jsch-agent-proxy)
    * See `examples/JSchWithAgentProxy.java` for simple example
    * ssh-agent support requires either [Java 16's JEP 380](https://openjdk.java.net/jeps/380) or the addition of [junixsocket](https://github.com/kohlschutter/junixsocket) to classpath
    * Pageant support is untested & requires the addition of [JNA](https://github.com/java-native-access/jna) to classpath
  * Added support for the following algorithms with older Java releases by using [Bouncy Castle](https://www.bouncycastle.org/java.html):
    * ssh-ed25519
    * ssh-ed448
    * curve25519-sha256
    * curve25519-sha256@<!-- -->libssh.org
    * curve448-sha512
    * chacha20-poly1305@<!-- -->openssh.com
* [0.1.65](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.65)
  * Added system properties to allow manipulation of various crypto algorithms used by default
  * Integrated JZlib, allowing use of zlib@<!-- -->openssh.com & zlib compressions without the need to provide the JZlib jar-file
  * Modularized the jar-file for use with Java 9 or newer
  * Added runtime controls for the min/max/preferred sizes used for diffie-hellman-group-exchange-sha256 & diffie-hellman-group-exchange-sha1
  * Renamed PubkeyAcceptedKeyTypes config to PubkeyAcceptedAlgorithms to match recent changes in OpenSSH (PubkeyAcceptedKeyTypes is still accepted for backward compatibility)
  * Reduced number of algorithms that are runtime checked by default via CheckCiphers, CheckMacs, CheckKexes & CheckSignatures to improve runtime performance
  * Added config options dhgex_min, dhgex_max & dhgex_preferred to allow runtime manipulation of key size negotiation in diffie-hellman-group-exchange type Kex algorithms
    * Default values are:
    * dhgex_min = 2048
    * dhgex_max = 8192
    * dhgex_preferred = 3072
* [0.1.64](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.64) Fixing [#55](https://github.com/mwiede/jsch/pull/55)
* [0.1.63](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.63) Fixing [#42](https://github.com/mwiede/jsch/issues/42)
* [0.1.62](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.62) bugfixes and code cleanup
* [0.1.61](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.61)
  * Add support for chacha20-poly1305@<!-- -->openssh.com, ssh-ed25519, ssh-ed448, curve448-sha512, diffie-hellman-group15-sha512 & diffie-hellman-group17-sha512. This makes use of the new EdDSA feature added in [Java 15's JEP 339](https://openjdk.java.net/jeps/339). [#17](https://github.com/mwiede/jsch/pull/17)
  * added integration test for public key authentication [#19](https://github.com/mwiede/jsch/pull/19)
* [0.1.60](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.60)
  * support for openssh-v1-private-key format [opensshFormat.md](opensshFormat.md).
  * Fix bug with AEAD ciphers when compression is used. [#15](https://github.com/mwiede/jsch/pull/15)
* [0.1.59](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.59) fixing issue from https://sourceforge.net/p/jsch/mailman/message/36872566/
* [0.1.58](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.58) support for more algorithms contributed by [@norrisjeremy](https://github.com/norrisjeremy) see [#4](https://github.com/mwiede/jsch/pull/4)
* [0.1.57](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.57) support for rsa-sha2-256 and rsa-sha2-512. [#1](https://github.com/mwiede/jsch/pull/1)
* [0.1.56](https://github.com/mwiede/jsch/releases/tag/jsch-0.1.56) support for direct-streamlocal@<!-- -->openssh.com (see [SocketForwardingL.java](examples/SocketForwardingL.java))
