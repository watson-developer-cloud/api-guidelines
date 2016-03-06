# Watson Developer Cloud GitHub Repository Guidelines

(These guidelines are for internal IBM teams looking to host projects on this GitHub page. We host the guidelines publicly here to give developers an idea of what to expect from our projects.)

So you’d like to host your project in the Watson Developer Cloud GitHub page! To help create a more useful developer experience, we have a set of guidelines to follow. Woo, guidelines, yeah!

1) Relevance

We aim for https://github.com/watson-developer-cloud to be the central repository for SDKs, demos and sample projects for the Watson Developer Cloud developer community. If your project is relevant to this community then this GitHub page may be the right place. Projects relevant only to specific users or events may be better suited to a more targeted page.

2) Naming and code quality

Relevant projects should follow the established conventions for naming of projects, packages, class names and other code artifacts. For example:

* The word “Watson” is included in the organization name and should not be in individual project names
* **We avoid abbreviations** and use dash separators for words in project names (so avoid “sst”, “movieapp”, etc.)

In additional to naming guidelines, we also have overall [code quality guidelines](code-style.md): Projects hosted in the Watson Developer Cloud GitHub should follow best practices in terms of language conventions, packaging, licensing, code style and readability.

To maintain this quality, review each project before adding it — you can send a link to an internally hosted project to [germanatt@us.ibm.com](mailto:germanatt@us.ibm.com) and [jsstylos@us.ibm.com] (mailto:jsstylos@us.ibm.com) and can expect a list of suggestions, especially related to naming.

3) Maintenance and deprecation plan

Even well written, relevant projects require continual maintenance to address user-submitted issues, patch bugs, keep to date with new service releases and dependency versions. By hosting a project on the Watson Developer Cloud GitHub, you’re committing to perform this maintenance for as long as the project remains hosted.

A common problem is that teams will focus on creating a project (such as a sample app), release it on GitHub, and then shift focus to a new project without the resources the maintain the old project. When this sample app breaks — because of a new service release or breaking dependencies — or when users submit issues that get no response, the overall quality and reputation of the entire Watson Developer Cloud GitHub organization is diminished.

To avoid this problem, we require that that teams commit resources for maintenance, and then when they cannot the projects get marked as deprecated and eventually removed, even if they are still needed by the sales team or for demos. **Projects that are still needed but cannot be maintained must be hosted elsewhere.**

4) Alternatives

If any of these conditions cannot be met, this doesn’t mean your project cannot be released — it just means it can’t be hosted on the Watson Developer Cloud GitHub page. It could still be hosted on JazzHub or a different GitHub organization or personal page. Hosting on a different GitHub page is encouraged for projects that aspire to be in the Watson Developer Cloud but need a little cleanup work — we can often review projects as they mature and move them to the Watson Developer Cloud page when they’re ready.
