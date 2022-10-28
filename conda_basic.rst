Starting to use conda
=====================

Mira Sohn

2022-10-27

I used to get questions from my collaborators about how to resolve dependency conflics when installing bioinformatics tools. Sometimes, bioinformatics projects require more than hundred different packages for a single project or analysis. Therefore, it's hardly possible to manually figure out compatibility of every software. SMy answer is, which is also the most straightforward way, to use `Conda <https://docs.conda.io/en/latest/>`_. According to the documentation, Conda is `Package, dependency and environment management for any language—Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, Fortran, and more`. When starting a new bioinformatics project, what I only need to do is to write tool names in ``requirements.txt`` file and run ``conda create --file requirements.txt``. 


I've been using Conda since 2020. As a relatively new user, I'd like to summarize a few basic Conda skills which I use in my bioinformatics projects.



