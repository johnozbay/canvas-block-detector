# Canvas Block Detector
A tiny, handy utility function to detect if the browser canvas is blocked either by using `privacy.resistFingerprinting` in Firefox, or by an extension / plugin.

---

Soooo I was building [Cryptee](https://crypt.ee), a cross-platform no-knowledge encrypted storage & privacy service for private documents, notes, journals, photos and more.
Everything is encrypted on user's devices with their encryption keys, and no-one, not even Cryptee can see users' data. Cool right? 😎
 
 
To achieve this no-knowledge system, Cryptee crops and generates photo thumbnails on the client side, using the browser canvas, before encrypting and uploading photos.
(Unlike unencrypted services. They handle all this resize / crop magic on their server.)

Now...

Cryptee's clientele/users are particularly smart and sensitive about their online browsing habits. 
They use all sorts of plugins, extensions, modified browsers, tailor-made operating systems, hand-soldered phones, and I bet some even live in nuclear bunkers.
And I have an immense amount of respect for my users. I've learnt so much from them, and they're simply incredible.

Some of the extensions Cryptee's users use, block access to browser canvas to stop malicious sites from fingerprinting and tracking visitors across the internet.
Or some users toggle Firefox's built in `privacy.resistFingerprinting` flag, which blocks access to the canvas.
 
Which is all great for privacy and anonymity while surfing the internet. 🌎

But it also sucks if a website like Cryptee is trying to use the canvas with good intentions, to actually **make the internet a safer and more private place**.

&nbsp;

The part that gets to me the most is, none of these extensions / browsers actually expose a boolean value for this feature in Javascript. So that web developers can detect if canvas is being blocked or not, and maybe show a banner to their users, if they need access to canvas.

So in case of Cryptee, users would upload photos, (often gigabytes of them), Cryptee would resize & crop the photos using canvas, thinking all is well, and after the upload is complete, users would find that all their thumbnails and lightbox preview images are all 100% white.

After countless user complaints about all-white thumbnails, I've found out the following nerve-wrecking fact. Turns out if you're using one of these blockers, [canvas actually **silently** returns all white pixels](https://bugzilla.mozilla.org/show_bug.cgi?id=967895#c126).
Yeah. **Silently.** Which, if you think about it, kinda makes sense. 

If they (browsers / extensions) did expose this as a boolean variable like : `canvasBlocked`, then some asshole on the internet would go ahead and use **that exact variable** to start tracking and fingerprinting users. 
And in return, exposing this would actually make these users with the canvas blocker more unique, defeating the whole purpose of this defense-against-fingerprinting.

&nbsp;

So yeah... It's worth talking about the privacy implications of a canvas fingerprinting countermeasure like these blockers.

Very few users toggle the hidden `privacy.resistFingerprinting` flag in Firefox or use an extension like Canvas Defender (around 40k as of this writing if you check the download statistics on Chrome's Extensions Store) 
Meaning that if you're detected with a blocked canvas, this in and of itself would make you A LOT more unique. (3bil internet users / 40,000 blocked canvases. You do the math.) 

While I get that the mission here is to increase user privacy by blocking the canvas, this can easily be used against the users to track them more easily, unless this technique is deployed by default on all browsers. Which they can't. *Because it's the fucking canvas.* It's a part of the [HTML spec](https://www.w3.org/TR/2dcontext/), we live in a post-Flash world and almost every web-based game uses the canvas nowadays. 

As someone who was trying to make good use of the `canvas`, to provide users a privacy service, this whole blocked canvas situation meant that I had to find a way to show my users a fucking ugly and ironic banner to turn off their "privacy extension", and to do so, first I had to find a way to detect if the canvas is blocked.

Which is exactly why this page & solution exists. 

I hope that this saves you, fellow developer, a week of head scratching. ✌🏻
