# Bionic & Meet the team

**Date:** 14, March, 2021

**Author:** Dhilip Sanjay S

---

## Initial Recon
- Initial clue - VCS being revealed.

```html
<!-- Vela, can we please stop sharing our version control software out on the public internet? -->
```

- So immediately I tried to access `https://constellations.page/.git`, but it was **Forbidden**.

```html
Forbidden
You don't have permission to access /.git/ on this server.
```

- But the access to files like `/index`, `/HEAD` were successful. `/logs/HEAD` folder:
```bash
0000000000000000000000000000000000000000 1142cc3145fdba8d9eb8f9c9e7ee79bdfda64d9a Leo Rison <leo.rison@constellations.page> 1614124430 -0500	commit (initial): Added initial assets and landing page
1142cc3145fdba8d9eb8f9c9e7ee79bdfda64d9a 87b17a86409582c162e260795afdf104dc1d46b1 Leo Rison <leo.rison@constellations.page> 1614124968 -0500	commit: Added the Meet The Team page
87b17a86409582c162e260795afdf104dc1d46b1 8e9e7afad5d1f7c6c3dcf322a3a94aeebc1e0073 Leo Rison <leo.rison@constellations.page> 1614125173 -0500	commit: Management said I need to remove the team details so I redacted that page and added it to robots.txt
8e9e7afad5d1f7c6c3dcf322a3a94aeebc1e0073 87b17a86409582c162e260795afdf104dc1d46b1 Leo Rison <leo.rison@constellations.page> 1614125488 -0500	checkout: moving from master to 87b17a
87b17a86409582c162e260795afdf104dc1d46b1 0780dea9ede681b1e4276d74740bb11056d97c39 Leo Rison <leo.rison@constellations.page> 1614125881 -0500	commit: Management said I need to remove the team details so I redacted that page and added it to robots.txt
0780dea9ede681b1e4276d74740bb11056d97c39 87b17a86409582c162e260795afdf104dc1d46b1 Leo Rison <leo.rison@constellations.page> 1614125918 -0500	checkout: moving from 0780dea9ede681b1e4276d74740bb11056d97c39 to 87b17a86409582c162e260795afdf104dc1d46b1
87b17a86409582c162e260795afdf104dc1d46b1 1142cc3145fdba8d9eb8f9c9e7ee79bdfda64d9a Leo Rison <leo.rison@constellations.page> 1614125954 -0500	checkout: moving from 87b17a86409582c162e260795afdf104dc1d46b1 to 1142cc
1142cc3145fdba8d9eb8f9c9e7ee79bdfda64d9a 4c88ac1c56fe228267cf415c3ef87d7c3b8abd60 Leo Rison <leo.rison@constellations.page> 1614125972 -0500	commit: Added the Meet The Team page
4c88ac1c56fe228267cf415c3ef87d7c3b8abd60 e7d4663ac6b436f95684c8bfc428cef0d7731455 Leo Rison <leo.rison@constellations.page> 1614126014 -0500	commit: Management said I need to remove the team details so I redacted that page and added it to robots.txt
```

## Searching for Tools
- Googled for some automated tool to download the `.git` folder.
    - [GitHacker](https://github.com/captain-noob/GitHacker)
    - [GitHack](https://github.com/captain-noob/GitHack)
    - [GitTools](https://github.com/internetwache/GitTools) - Worked for this scenario.

## Looking for the appropriate git commit
- By running `git log --patch` inside the git folder, we find two flags:
    - One flag in **meet-the-team.html:** flag{4063962f3a52f923ddb4411c139dd24c}

    ```html
    <!-- Projects Section -->
      <section id="projects" class="projects-section bg-light">
             <div class="container">
                       <ul>
                               <li><h4><b>Orion Morra</b> &mdash; Support</h4></li>
                               
                               <li><h4><b>Lyra Patte</b> &mdash; Marketing</h4></li>
                               
                               <li><h4><b>Leo Rison</b> &mdash; Development</h4></li>
                               
                               <li><h4><b>Gemini Coley</b> &mdash; Operations</h4></li>

                               <li><h4><b>Hercules Scoxland</b> &mdash; Sales</h4></li>

                               <li><h4><b>Vela Leray</b> &mdash; Management</h4></li>

                               <li><h4><b>Pavo Welly</b> &mdash; HR</h4></li>

                               <li><h4><b>Gus Rodry</b> &mdash; Accounting</h4></li>

                               <!-- <li><h4><b>flag{4063962f3a52f923ddb4411c139dd24c}</b></h4></li> -->

                       </ul>
               </div>
       </section>    
    ```

    - Another flag in **robots.txt:** flag{33b5240485dda77430d3de22996297a1}

    ```
    +User-agent: *
    +Disallow: /meet-the-team.html
    +
    +flag{33b5240485dda77430d3de22996297a1}
    ```
    



## References
- [Source code exposure via source code disclosure](https://medium.com/@roshancp/source-code-disclosure-via-exposed-git-folder-d22919c590a2)
- [What's inside your git directory](http://gitready.com/advanced/2009/03/23/whats-inside-your-git-directory.html)
- [John Hammond - Git Happens](https://www.youtube.com/watch?v=3iGZMMga5pI)
    - Useful git commands: 
        - `git log --patch`
        - `git log | grep commit | cut -d " " -f2 | xargs git show`