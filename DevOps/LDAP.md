# linux 的秘密都在 PAM 里

大家熟练使用 `journal -x` debug 各种系统用户认证的各种状态机的时候，抑或是 sshd 时，可以看到 PAM、xsecurity、sssd 这种字眼，这是用户认证协议。SSSD 就是一个 daemon，把系统的 NSS PAM 的机制和 LDAP 连接起来。（之前 20.04 gnome 被打也是这个协议被绕过的结果。

熟悉了 PAM 之后，你也就知道为毛 `~/.ssh/id_*.pub` 需要 `r..`，这写死在 nss 用户目录协议里了。

继续阅读：https://jia.je/software/2021/02/15/sssd-ldap/

## ref
- [RFC2307 An Approach for Using LDAP as a Network Information Service](https://tools.ietf.org/html/rfc2307)
- [RFC3062 LDAP Password Modify Extended Operation](https://tools.ietf.org/html/rfc3062)
- [RFC4511 Lightweight Directory Access Protocol (LDAP): The Protocol](https://tools.ietf.org/html/rfc4511)
- [RFC4512 Lightweight Directory Access Protocol (LDAP): Directory Information Models](https://tools.ietf.org/html/rfc4512)
- [RFC4513 Lightweight Directory Access Protocol (LDAP): Authentication Methods and Security Mechanisms](https://tools.ietf.org/html/rfc4513)
- [RFC4517 Lightweight Directory Access Protocol (LDAP): Syntaxes and Matching Rules](https://tools.ietf.org/html/rfc4517)
- [RFC4519 Lightweight Directory Access Protocol (LDAP): Schema for User Applications](https://tools.ietf.org/html/rfc4519)

