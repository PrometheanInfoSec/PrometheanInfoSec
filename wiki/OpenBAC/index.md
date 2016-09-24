#OpenBAC

##Website
https://github.com/prometheaninfosec/openbac

##Description
OpenBAC is an open source implementation of the Ball and Chain password storage algorithm.

If you're curious as to what Ball and Chain is, please watch this conference
talk describing it in full.
https://www.youtube.com/watch?v=GfyM8lFkjo8&feature=youtu.be

Long story short, Ball and Chain ties authentication to you network itself using large collections of random data. These large collections of random data are incredibly hard for an attacker to extract, making it next to impossible for him to ever perform and offline attack against your user's passwords.

Think of it, kind of like tying down the stuff in your backyard before a hurricane. We assume the wind is coming. We harden ourselves against it. It doesn't matter if the attacker can gain access to your network. Even with root level control over the stored passwords, he still can't crack them (if implemented correctly).

You can use this library, and it's utilities to implement Ball and Chain is your next application (web or otherwise). Your users will thank you.

##Documents

- [Library Overview](openbac.md)
- [Server Overview](server.md)
- [Under the Hood](under_the_hood.md)
- [Usage Example](usage_example.md)
- [On Password Complexity](on_password_complexity.md)
- [Interactive-Generate](interactive-generate.md)
- [Misc](misc.md)



