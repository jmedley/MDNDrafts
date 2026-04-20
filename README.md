# MDNDrafts

This repo is for drafts of non-standard APIs as part of my [AllBrowsers](https://josephpmedley.wordpress.com/) project. So far, the going is slow because my need to conserve tokens for my job search.

The process form draft specification to web platform feature is lengthy, and only the final stage is typically documented on [MDN Web Docs](https://developer.mozilla.org/en-US/). This leavs a hole in the process of evolving the web platform that was not practical to fill before AI, at least, not practical with more labor. APIs in their testing phase are typically not documented because it's difficult to draft reference pages up-to-date with change to the spec. With AI, these change can be made in seconds rather than days or weeks. 

Many cutting edge APIs are given minimal documentation in articles published on the web sites of browser vendors. For reasons already mentioned, these articles are frequently minimal and incomplete. That predisposes all users to a tested path, ensuring that blind spots in the design and bugs in the implementation are overlooked until, possibly even when they become formal standards.

## The State of the Repo

Currently the generated contents of this repo, merely mirror the structure of MDN source pages. As time and AI tokens allow, I plan to make these pages more usable.  

## The Prompt

Here's the prompt I use to generate the pages in this repo.

```You are a technical writer.

Instructions:

1. With a specification I will give you, extract the IDL from WebSpec.
2. Using the IDL, identify all APIs. Ignore anything marked as Deprecated or Experimental.
3. For each API, write an MDN-style page using the appropriate item from Models. Write the page in Markdown and save it to a file.
4. Provide me with links to download the files. 

APIs: callback, dictionary, interface, method, property

Models:
callback: https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/htmlvideoelement/requestvideoframecallback/index.md
dictionary: https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/requestinit/index.md
interface: https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/networkinformation/index.md
method: https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/request/arraybuffer/index.md
Navigator attribute (property): https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/navigator/connection/index.md
property: https://raw.githubusercontent.com/mdn/content/refs/heads/main/files/en-us/web/api/htmlelement/innertext/index.md```
