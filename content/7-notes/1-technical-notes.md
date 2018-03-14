# Technical Research Notes

*written by [@jmatsushita](https://gitlab.com/jmatsushita) as issue [#1](https://gitlab.com/contentascode/activist-apprentice/issues/1)*

---

## React Native vs Ionic.

I spent a few hours kicking the tires of a range of react-native tools and looked back at the Open Mentoring code and I can say that it's not worth picking up the previous code base and won't really have an impact on timing.

I conducted an initial research phase in order to evaluate React Native, compared to my previous experience of Ionic (See more [below](#react-native-vs-ionic)). Here's a very good breakdown if you have any questions https://www.codementor.io/fmcorz/react-native-vs-ionic-du1087rsw

I have found that the toolset is mature and flexible enough to server our purpose. I've set aside the possibility to use Expo, the now default option proposed when creating a React Native app, which is a very attractive option during development as it allows previewing apps without installing the Android SDK and allows to host apps for team review. However it also update apps over the air through their own cloud infrastructure which creates an infrastructure lock-in and creates a dependency that we don't want to tie ourselves to both on the server side and client side.

Minimum Android version supported by React Native is 4.1 (API 16).

---

## App Previewing

Using regular react-native, while requiring some additional configuration (for the developer team), still offers very convenient development options with live reload, remote debugging and even HMR. It also enables using react-native modules that extends the possibilities to use different client database or storage options, or possibily other android Java based modules (such as PanicKit).

It could be useful to keep an Expo stripped down version of the app to allow Publishers to see rapidly the results of the content work and theming on their phone without the overhead of installing all the Android software kit, or the wait for the server to generate the app, or the approximation that a local browser based version would have.

---

## Theming

This proposal assumes that we'll be using a "simple" UI kit (in this case NativeBase) without customisation of UI components beyond theming colors, fonts and logos. The other aspects that will be configurable is the About page. Unfortunately NativeBase doesn't have out of the box RTL support yet (it's on [their roadmap though but without a date estimate](https://github.com/GeekyAnts/NativeBase/issues/518)). I've made a estimation for implementing basic RTL for only the components we used but I haven't included it in the minimum proposal. That would only be needed if we required an RTL app before NativeBase implements it.

---

## Course Storage

I need to do more research on the way that courses are stored within the app. If they are baked into the APK as static asset files then, although it makes a number of things easier, they won't be deletable from the panic option, unless maybe if the whole app is uninstalled. Using the built-in database seems that it would be quite limited in terms of performance and also because storing images inside it would be a problem. There are react native module that give access to the file system.

There are database module options (sqlite, realm) which could possibly also store binary blobs, some of which could offer some full text search options. There might be options to use an OBB, and I'll research if its possible to just delete them from our code and how it would impacts packaging and distribution.

This also needs to consider the question of minimising bandwidth usage for the simple content update use case (for which file system text patches alongside some form of media blob transfer would be most efficient).

---

## Panic Feature

The approach proposed in the minimum approach is to rely on the sibling project to delete the Course app completely without having to deal with deletion within the Course app itself.

I've still costed as an option a built in panic feature which would delete all course content when selected. This could then allow to use this feature as a Panic Kit receiver during later work. Panic Kit integration would require developing a link with the react-native framework. If this is developed as a react-native module (installable via react-native link) then this could be an important contribution to the android ecosystem, opening up the possibility to include Panic Kit features for all react-native apps. If this was a further direction of work that SWN was interested in sponsoring, then iilab would be willing to take on the maintenance of the module.

---

## System Infrastructure

With the mimimum approach, the infrastructure needed would be a virtual host for a RepoMaker instance. The rest of the infrastucture should be able to piggy back on existing public services (Gitlab, Github, Travis, F-Droid).

I haven't included the costs which should only require a small VM, so you should keep this in mind (cost and maintenance). We might be able to use Greenhost's Eclipsis during development. If we'd go for this option, then I'd recommend asking them asap so we could start using it now.

Although this option is not included in the minimum proposal, the ideal APK generation infrastructure would require this type of architecture:

 - Use the public Gitlab to host the repo (optionally with a Github mirror).
 - Use Gitlab pages (or Github pages) to host web based previews.
 - Use a self-hosted "integration" server which will orchestrate various aspects of the app testing, building and publishing.
 - Use a self-hosted version of fdroidserver, since there would be limitation of build time or our capacity to access non-public features or custom made features on the public F-droid server instance. (there could be an option where this is built by the public F-Droid server instance or even via Travis CI)
 - Use a self-hosted RepoMaker instance for APK publishing (with the integration server posting updates to it when APKs are built).

This larger option means that having a medium size server or VM to run these self-hosted components would be necessary.

---

## Basic Security considerations

Some of my initial thoughts here, relate to the possibility of tampering with the content publication and app generation pipeline. In other words guaranteeing that the publication process cannot be highjacked is important for end-user security.

Reproducible builds seem like a possibly achievable objective although this hasn't been costed at this point. It would enable a better app version upgrade UX. Ensuring all apps distributed have the same developer signing keys mean they can update even if the update comes from different publishing platforms, the potential failure case that this helps address is that for instance a bluetooth distributed udpate for

Other aspects to keep in mind and that will need further thought relate to content storage and secure deletion for the Panic feature.

---
