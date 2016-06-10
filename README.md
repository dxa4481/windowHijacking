# Window hijacking

This is a demonstration of a website opening a new tab after a link is clicked, and then after a timer of any length, while the user is on the new page, changing the location of that new page.

## Context
It's known that setting a <a> tag with a target attribute as _blank has security risks:

https://mathiasbynens.github.io/rel-noopener/

This is because the newly opened page has the ability to change the window location of the page that opened it, with the following:

```
window.opener.location = "https://google.com"
```

However this demonstration shows a website has the ability create a new page in a new tab, and then change the location of the newly created page after an arbitrary peroid of time has passed. This can be achieved as follows

```html
<script>
    var windowHijack = function(){
        window.open('https://legitloginpage.xyz', 'test');
        setTimeout(function(){window.open('https://notlegitloginpage.xyz', 'test');}, 300000);
    }
</script>
<button onclick="windowHijack()">Open Window!</button>
```

In the above example, a new window is opened when the button is pressed, and 5 minutes later, the new window will change locations. **Even if the new tab is changed to a new website, or refreshed, the original website can still change the location**

## Impact
Users may be tricked into clicking links that are innocent, but change to be malicious after an arbitrary period of time. For example, a link to _facebook.com_ may take a user to facebook, however after an arbitrary peroid of time, the _facebook.com_ tab may change to _faceobok.com_ and present a user with a fraudulent cloned login page to steal credentials.

## Demo 1
In this example, a legitimate login page is linked, and the timer is set to 5 seconds. When the timer expires, the legitimate login page is changed to an illegitimate login page which has a keylogger installed on it.

https://security.love/windowHijacking

## Demo 2

In this secondary example, the attack is combined with [Pastejacking](https://github.com/dxa4481/Pastejacking). A legitimate _serverfault.com_ question is linked. After being opened, a 5 second timer will change the location of the legitimate serverfault website to a malicious clone of the original serverfault page, with pastejacking code installed. This causes any user who tries to copy the answer to get "cat /etc/passwd\n" injected into their clipboard.

https://security.love/windowHijacking/index2.html

## Other considerations
When performing this attack, the opened page also has the ability to also change the location of the parent page. This can be accomplished by the same _window.opener_ method shown above for _blank links. This can be used to stop JavaScript timers on parent pages.
