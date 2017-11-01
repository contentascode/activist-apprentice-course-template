---
layout: index.pug
---

## 1.2 COMMUNICATE SAFELY

Whether you are doing reporting, documenting your friends, or promoting a political movement, you may be at risk from electronic observation. Technical or electronic safety depends on your communications. You should observe proper security practices in all forms of electronic communication.

---

# KNOW YOUR CONNECTION.

# IF YOU DON'T CONTROL IT, DON'T TRUST IT.

---

If you are using a computer that is not your own, assume everything can be observed even if it says it is secure (https, encrypted, etc). Keyloggers, false SSL certificates, and other monitoring software are all easy to install.

Never post something from work computer or an Internet cafe computer and expect it to be anonymous - ever. Use your own PC or laptop and boot from a live cd if possible.*

*We advise you use ubuntu to use as an easy option for using a live cd.*
http://www.ubuntu.com/download/ubuntu/download

## PHONE

Each phone has a unique identity. Unless you obtain an anonymous phone, nothing you say is private. To communicate privately, make calls only between anonymous phones.  

A measure of security is possible over VOIP clients like Skype but is by no means 100% secure.

## SMS

SMS is searchable and indexable. This makes unencrypted SMS easy to monitor, so ideally send only encrypted SMS, and send them minimally and only between anonymous phones.

A simple system to avoid security breaches is to create a coded communication system with trusted colleagues. Build up a collection of codes to relay basic information.

For example, “X” could mean you’re in danger, and “Z” could mean you’re safe. “Tree” could mean you want to meet up. “Banner” could be a cafe you can meet at.

It’s up to you, however, to figure out a system that is easy for you and your contacts to remember.

There are third party software solutions for secure texting, but they’re not as easy to control as a personal SMS representative system. You might use **SMS 007**, **TigerText**, **TextSecure**, **CryptoSMS** to text securely. (This requires Java support on your phone.)

## EMAIL

Use email that supports **HTTPS** secure connections. We advise using **HushMail** or **Gmail** for this.

It’s important to be sure you have a secure connection. When connecting to email services, do not connect via **HTTP://** -- connect only via **HTTP*S://***. You can set this to the default option in Gmail by going to the settings and selecting “Always use HTTPS”. You should also make sure anyone you communicate is doing this.

Do not open attachments. Use the “view” option instead.

For additional anonymity, communicate only between new anonymous email accounts. You can also share one anonymous email account and save all messages as drafts, so that the content never leaves your account.

Use a different browser just for anonymous use and use it only for that. Reset the browser before and after each use (be certain to erase browsing history and remove cookies).

For added security, connect from a different IP address when doing things anonymously. For example, use your regular accounts when at home but use your anonymous accounts only when on wireless at a cafe.

## CHAT

**OTR** (Off the Record) (Off the Record) is an important plug-in feature to look for in chat clients. We suggest you use **Pidgin**, but be sure to install the OTR plug-in after downloading it.

You can also use the OTR setting in the chat client provided with Gmail: Settings/Chat/Never Save Chat History

## WEB BROWSER

# The only way you are secure is if you trust your computer and know it’s software isn't compromised.

### SSL Certificates Incorrectly installed


**example 1:** SSL certificate is misconfigured and allows me to use *webmail.google.com.securewebsite.com* to instead of *webmail.google.com* to convince you to give me your password. The poorly designed site certificate means everything looks OK when I check, unless I notice the URL is wrong.

---

### Site Owners Willing to Give Out Information

**example 1:** I use webmail over HTTPS, webmail company complies with government request and hands over access to my account.

**example 2:** I use webmail, the totalitarian regime where I live hacks my webmail account password or the password of someone I emailed and extracts my information.

### Compromised networks

**example 1:** You use a computer at an Internet cafe, the system administrator adds his own server to the web browser's trusted authorities list and pushes a SSL certificate for all websites. As your information passes through the transparent network proxy, the information from your webmail provider is decoded and re-encoded without you ever knowing - you get a green security lock and no warnings, in fact if you check the certificate it says everything is great. In the meantime, the administrator is copying and reading everything that you do.

**Best Practices:**

Nothing is foolproof, but by installing the latest version of **Firefox** on a computer you trust you’ll be much less susceptible to monitoring.

If you are prompted to give a security exception or there is an error or x on the https or lock icon beware!

Be sure to always use HTTPS in *“Settings/General/Browser Connection.*
