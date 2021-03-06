This repository provides a minimalistic base template for creating a hands-on workshop
following the framework set out in the Bioinformatics Training Platform (BTP). Below we have
provided various entry-points, by way of workflows, to demonstrate the various ways in which you
can utilise existing workshops and workshop modules. We also provide information on how to go about
developing your own modules and workshops.

Table of Contents
=================
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Prerequisites](#prerequisites)
  - [Installing Hub](#installing-hub)
- [Basic Concepts](#basic-concepts)
  - [Workshop Modules](#workshop-modules)
  - [Workshops](#workshops)
  - [Fork and Pull Collaborative Model](#fork-and-pull-collaborative-model)
- [General Workflows](#general-workflows)
  - [Reusing an Existing BTP Workshop for Self-Directed Learning](#reusing-an-existing-btp-workshop-for-self-directed-learning)
  - [Reusing an Existing BTP Workshop to Run Your Own Workshop](#reusing-an-existing-btp-workshop-to-run-your-own-workshop)
  - [Developing Your Own BTP Workshop from Existing Modules](#developing-your-own-btp-workshop-from-existing-modules)
  - [Developing Your Own BTP Modules](#developing-your-own-btp-modules)
  - [Developing Your Own BTP Workshop from Scratch](#developing-your-own-btp-workshop-from-scratch)
  - [Updating an Existing Workshop](#updating-an-existing-workshop)
    - [Updating a Module](#updating-a-module)
    - [Updating a Workshop](#updating-a-workshop)
- [Advanced Workshop Customisations](#advanced-workshop-customisations)
  - [Minting a DOI for your Workshop](#minting-a-doi-for-your-workshop)
  - [Customise the Handout Styling](#customise-the-handout-styling)
  - [Use Travis-CI to automate PDF Building](#use-travis-ci-to-automate-pdf-building)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

Prerequisites
=============
We assume you are working on a Linux OS and have command line experience, or at least you're not
scared by it! The commands provided in the [General Workflows](#general-workflows) sections have
been written in bash for a 64-bit Ubuntu OS but should work on other Linux flavours with little
modification.

Here is a list of software prerequisites:

- [**hub**](https://hub.github.com/): This tool provides command line access to GitHub so you can do
    things like create and fork repositories on GitHub via the command line.

Installing Hub
--------------
Download the latest release from https://github.com/github/hub/releases/latest. As at this time,
the lastest version is v2.2.1 and we can download and install hub using the following commands:

```bash
# Set the version we want to install
HUB_VERSION='2.2.1'
cd /tmp
# Download
wget "https://github.com/github/hub/releases/download/v${HUB_VERSION}/hub-linux-amd64-${HUB_VERSION}.tar.gz"
# Extract
tar xzf "hub-linux-amd64-${HUB_VERSION}.tar.gz"
# Copy the binary onto the path
sudo cp "hub-linux-amd64-${HUB_VERSION}/hub" /usr/local/bin/
```

Basic Concepts
==============
In order to help create a more reusable, plug-and-play like system for developing workshops we have
a few key concepts which should be understood in order to get the most out of the BTP system.

Workshop Modules
----------------
A workshop module is a major, self-contained component of a hands-on workshop. It can be reused,
in a mix-and-match way, along with other modules to put together a new workshop. Each module has
its own repository and contain all the required information for the teaching of that particular module's
content. They contain, LaTeX source code for the handout, presentation matrials to help introduce
concepts covered in the module as well as metadata describing the data and tools used in the
module excercies.

To start putting together your own workshop module, head over to the [btp-module-template](https://github.com/BPA-CSIRO-Workshops/btp-module-template)
repositoriy for further details.

Workshops
---------
Workshop repositories pull together 1 or more workshop modules and provide the necessary files to
glue them together into a single coherent workshop. There are two types of workshops repositores:

  1. An almost static workshop repository which captures the workshop which was run at a particular
  location on a paticular date. Think of this as a snapshot of a workshop which was run and maintained
  for posterity.
  2. A master workshop template which is updated and maintained over time. Think of this as a
  template from which the above static workshop repository is created.

Fork and Pull Collaborative Model
---------------------------------
We will assume that you are using a [fork & pull collaborative model](https://help.github.com/articles/using-pull-requests/#fork--pull)
to getting updates included into a workshop or workshop module. This means that the master
repository of a workshop or module has limited GitHub users which have write access and can thus
OK changes into a particular repository.

To modify who has write access to your GitHub repository, head over to the repository's
Settings >> Collaborators page. Whether the repository is under your personal space or an
organisation will determine exactly how youmake these changes. For full details of both
approaches, see the GitHub Help for
[adding collaborators to a personal repository](https://help.github.com/articles/adding-collaborators-to-a-personal-repository/)
or [permission levels for an organisation repository](https://help.github.com/articles/permission-levels-for-an-organization-repository/).

This provides a convienient way of controlling how changes are vetted before being included into a
module or workshop. Choose wisely which users you give this power to.

General Workflows
=================
The following workflows are to provide guidence on how to achieve particular tasks; from updating
a workshop module to writing your own workshop from scratch.

Reusing an Existing BTP Workshop for Self-Directed Learning
-----------------------------------------------------------
TODO

Reusing an Existing BTP Workshop to Run Your Own Workshop
---------------------------------------------------------
The easiest way to get started is to use an existing Bioinformatics Training Platform (BTP)
workshop. These workshops are like a master template for a given workshop; they are cloned
in order to run a new workshop of the same kind and are maintained and updated over time.

For the purpose of this example workflow we are going to use the [btp-workshop-ngs]
(https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs) repository. To see a list of available
BTP workshops head over to: https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-

  1. Clone the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
  repository into a new repository on GitHub. We'll create a repository under our username called
  `ngs-workshop_SYD-2015-05` so we know exactly to which workshop the repository pertains. The idea
  is that once the workshop has been run, this repository will remain unchanged. As such it will
  always provide a snapshot of what was covered during a particular workshop.

  ```bash
  NEW_REPO_NAME='nathanhaigh/ngs-workshop_SYD-2015-05'
  NEW_REPO_DESC='NGS Workshop: Sydney May 2015'
  
  cd /tmp/test
  git clone --recursive "https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs.git" ngs-workshop_SYD-2015-05
  cd ngs-workshop_SYD-2015-05
  
  hub create -d "${NEW_REPO_DESC}" "${NEW_REPO_NAME}"
  git remote set-url origin git@github.com:nathanhaigh/ngs-workshop_SYD-2015-05.git
  git push
  ```

  2. You should now customise the repository to reflect your workshop-specific details.
    1. `README.md` - Travis and zenodo badges, workshop info content
    2. `template.tex` - Modify `\setWorkshopTitle`, `\setWorkshopVenue`, `\setWorkshopDate` and
    `\setWorkshopAuthor` to reflext the specific of your workshop. These will be placed into the
    handout.
    3. `010_trainers/` - Delete unnecessary trainer photos from `010_trainers/photos/` and add
    photos of your own trainers instead. Modify `010_trainers/trainers.tex` to contain only yor own
    trainers and use the photos you placed into `010_trainers/photos/` or used the
    `010_trainers/generic.jpg` image for camera-shy trainers.
  3. Build your trainee and trainer handout PDFs.

```bash
# Perform a 1-time install of a minimal tex-live so you have everything you need to build the PDFs
# from the LaTeX source
cd ./developers/ && sudo -E ./texlive_install.sh && cd ../

# Build the trainee_handout.pdf and trainer_handout.pdf
PATH=/usr/local/texlive/bin/x86_64-linux:$PATH make
```

Developing Your Own BTP Workshop from Existing Modules
------------------------------------------------------
We use the [btp-workshop-template](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template)
repository as a template from which to create our brand new workshop. Because we know that this
new workshop will be in high demand, we'll set up and configure a master workshop repository. This
will then be used to create static, workshop-specific repositories. Our new workshop will be an
introduction to RNA-seq.

We start by looking at what [existing BTP modules](https://github.com/BPA-CSIRO-Workshops?query=btp-module-)
we might be able to reuse. Lets say we decide to only include the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc) and
[btp-module-rna-seq](https://github.com/BPA-CSIRO-Workshops/btp-module-rna-seq) modules.

We first create a local clone of the [btp-workshop-template](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template)
repository, add the two workshop modules as git submodules, ...

```bash
# Lets specify a name for our workshop
WORKSHOP_NAME='my_new_workshop'

# Clone the workshop template as our starting place
git clone --recursive git@github.com:BPA-CSIRO-Workshops/btp-workshop-template.git "${WORKSHOP_NAME}"
cd "${WORKSHOP_NAME}"

# Add the two workshop modules as git submodules
# Use a number prefix to the submodules directories to indicate the order in workshop
# The QC module will come first, followed by the RNA-Seq module, as designated by the 020
# and 030 prefix. Note they come after 010_trainers and 020_preamble.
git submodule add git@github.com:BPA-CSIRO-Workshops/btp-module-ngs-qc.git 020_qc
git commit -m "Added the QC module"
git submodule add git@github.com:BPA-CSIRO-Workshops/btp-module-rna-seq.git 030_rna-seq
git commit -m "Added the RNA-Seq module"

# Create the master workshop repository in your personal GitHub space using hub
hub create -d "A description of my new workshop"

# Update the origin remote of this local repository to point to our new master workshop
# repository on GitHub and push our new workshop to it. Change <USER> for your GitHub username
git remote set-url origin git@github.com:<USER>/${WORKSHOP_NAME}.git
git push
```

Rather than starting your workshop from the [btp-workshop-template](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template)
repository, you could also use an existing [btp-workshop-](https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-)
and start modifying it from there. If you take this approach, you are more likely to want to
remove workshop modules. You can do this very easily:

```bash
# Lets specify a name for our workshop
WORKSHOP_NAME='my_new_workshop'

# Clone the btp-workshop-ngs
git clone --recursive git@github.com:BPA-CSIRO-Workshops/btp-workshop-ngs.git "${WORKSHOP_NAME}"
cd "${WORKSHOP_NAME}"

# Delete all but the QC and RNA-Seq submodules
git rm 060_alignment
git commit -m "Deleted 060_alignment submodule"
git rm 070_chip-seq 090_velvet 905_post-workshop
git commit -m "Deleted other submodules"

# Create the master workshop repository in your personal GitHub space using hub
hub create -d "A description of my new workshop"

# Update the origin remote of this local repository to point to our new master workshop
# repository on GitHub and push our new workshop to it. Change <USER> for your GitHub username
git remote set-url origin git@github.com:<USER>/${WORKSHOP_NAME}.git
git push
```

Developing Your Own BTP Modules
-------------------------------
Head over to the [btp-module-template](https://github.com/BPA-CSIRO-Workshops/btp-module-template)
repository. It contains all the information and example info on how to put together your own
workshop module including:

  1. How to setup the directory structure of the repository
  2. How to write your handout content using LaTeX and our style file for achieving a consistent
  document styling

In the [example.tex](https://github.com/BPA-CSIRO-Workshops/btp-module-template/blob/master/handout/example.tex)
file You'll find lots of useful info and links to online resources to help you on your way with
writing LaTeX.

Developing Your Own BTP Workshop from Scratch
---------------------------------------------
TODO

Updating an Existing Workshop
-----------------------------
We will assume that you are using a [fork & pull collaborative model](https://help.github.com/articles/using-pull-requests/#fork--pull)
to getting updates included into a repository. Even if you have write access to a repository, we
advise that you resist making changes directly into the master repository or OK'ing your own
updates. This ensures there are stricter quality controls and check on what changes are made to
your master repositories.

Since each workshop module is a seperate git repository, any updates to a workshop means first
updating a workshop module.

### Updating a Module

To demonstrate this workflow we will use the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
repository as the example.

We won't make changes directly in the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
repository because we either don't have permission or our policy doesn't allow it. Instead we will
fork the repository, make our changes there and then [issue a pull request](https://help.github.com/articles/using-pull-requests/#initiating-the-pull-request)

Lets take this one step at a time and use the command line where possible rather than using the
GitHub website:

```bash
# Create a local clone of the repository we want to fork
cd /tmp
git clone --recursive https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc.git
cd btp-module-ngs-qc

# Create the fork using hub
hub fork

# Make a change and commit it to your local repository
touch test
git add test
git commit -m "Added test file"

# Push it up to your repository on GitHub - replace <USER> with your own GitHub username
git push --set-upstream <USER> master

# Subsequent pushes will only need a "git push"
touch test2
git add test2
git commit -m "Added test2 file"
git push

# Issue a pull request to request your changes be included into the module repository
hub pull-request -m "Test pull request"
```

Now wait until someone with write access to the module repository has merged in your changes
or otherwise provided a comment on your proposed changes. Once a decision has been made regarding
your merge request, you can delete your fork of the repository. To do this, simply head over to the
repository's settings page. For full details, see GitHub's
[deleting a repository](https://help.github.com/articles/deleting-a-repository/) help page.

To have an existing workshop repository utilise these updates you will need to update the workshop
repository's git submodules. This is detailed in the [Updating a Workshop](#updating-a-workshop)
section.

### Updating a Workshop

A workshop is comprised of 1 or more modules which are included in the repository as git submodules.
A submodule always points to a particular revision of the module repository; usually the revision
when the module was added as a submodule to the workshop repsitory. As such, if a module gets
updated the submodule is still pointing to the same (older) revision of the module repository. We
need to update this if we want the workshop to use the new and improved updates that have been
included in the module.

We assume that some update(s) have been added to the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
module and we now want to have those changes reflected in the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
workshop repository.

Remember, we're not allowed to make changes directly in the master [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
repository. We either don't have permissions to do that ourself or we want someone else to check
what we're doing! As such, we'll fork the workshop repository and issue a pull request.

```bash
# Create a local clone of the repository we want to fork
cd /tmp
git clone --recursive https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs.git
cd btp-workshop-ngs

# Create the fork using hub
hub fork

# Update a git submodule so it points to the latest revision of the workshop module
cd 050_ngs-qc
git pull origin master
cd ../
git add 050_ngs-qc
git commit -m "Updated QC submodule to latest revision"

# Push it up to your repository on GitHub - replace <USER> with your own GitHub username
git push --set-upstream <USER> master

# Update all submodules so they all point to the latest revisions in their
# respective repositories
git submodule update --remote
git add .
git commit -m "Updated all submodule to their latest revisions"

# Push it up to your repository on GitHub
git push

# Issue a pull request to request your changes be included into the module repository
hub pull-request -m "Updated all submodules to their latest revisions"
```

Advanced Workshop Customisations
================================
Once you've got the hang of making basic modifications of existing workshops, you'll find there are
other useful and interesting things you might like to customise. This section aims to guide you
though some of these.

Minting a DOI for your Workshop
-------------------------------
So, you've [developed your own BTP workshop](#developing-your-own-btp-workshop-from-scratch) and
you want to get credit for all your hard work or you simply want to have a public record of a
particular workshop so people can refer back to the content used. With a DOI, you can do just that.

In order to generate a DOI for your workshop repository, you first need to login to a service
external to GitHub, called [zenodo](https://zenodo.org/), and enable DOI generation for your
workshop repository. Then, when you create a new "release" of your repository, zenodo will store a
copy of the "release" for posterity and generate a DOI that will point to that copy held by zenodo.
This means that even in the event that GitHub goes away, there will be a copy of your repository as
it stood at the time you created the "release".

  1. Login to the [zenodo](https://zenodo.org/) website using your GitHub account details.
  2. Find the GitHub settings on zenodo; you should see a listing of all your GitHub repositories.
  3. Flick the switch to "ON" next to the repository for which you what zenodo to create DOI's
  whenever you make a "release" for that repository.
  4. Head back over to the repository on GitHub and click "releases" in the header of the repository.
  5. Click "Create a new release" and complete the form.
  6. Once the "release" is made, zenodo will detect this and do it's thing. Head back to the
  GitHub settings page on zenodo and you should find a DOI badge next to the workshop repository
  name containing the DOI. If you click the badge, you will be provided with code for inclusion in
  various file formats, including Markdown, HTML and a URL to an SVG image.

Full details of this process can be found on the [citable code](https://guides.github.com/activities/citable-code/)
GitHub help page.

Customise the Handout Styling
-----------------------------
TODO

Use Travis-CI to automate PDF Building
--------------------------------------
IN PROGRESS

Ideally, everyone who contributes LaTeX code to your module will test if the PDF can still be built from the source
without error, before the change is comitted to the repository. This of course means that each collaborator has a
working TeX environment such as [TeX Live](http://www.tug.org/texlive/) (multi-platform),
[MiKTeX](http://www.miktex.org/) (MS Windows), [proTeXt](http://www.tug.org/protext/) (MS Windows) or
[MacTeX](http://www.tug.org/mactex/) (Apple Mac). However, what if edits are performed online, the collaborator
couldn't get a TeX environment installed easily simply forgets to test the build before committing their changes? As a
result, you can find yourself with a LaTeX project which no longer compiles and debugging becomes time-consuming.

The Travis Continuous Integration ([Travis-CI](https://travis-ci.org/)) system can be instructed to perform almost any automated task following a commit being pushed to a GitHub repository. By utilising Travis-CI we can:

  1. Automate the PDF build process, thereby removing the requirement for all collaborators to have a working TeX
environment and to check the building before pushing changes.
  2. We are able to catch LaTeX errors early while they are easier to debug.
  3. We can publish the resulting PDF to the repository's [GitHub Pages](https://pages.github.com/).

### Configuring Travis-CI 
All that is required is a a bit of configuration to get Travis-CI and your GitHub repository talking and a special
`.travis.yml` file in the top level of your repository. This file is where you place instructions for Travis-CI to
perform. To enable Travis-CI to automate tasks associated with your GitHub-based LaTeX repository (I assume this already exists) we need to set up a few things first (full details at http://docs.travis-ci.com/user/getting-started/). These are:

  1. Sign in to [Travis-CI](https://travis-ci.org/) using your GitHub account credentials and OK the permissions
required by Travis-CI to access your GitHub account.
  2. Enable your LaTeX project repository on your Travis-CI [profile page](https://travis-ci.org/profile). Once this
is done, Travis-CI is then monitoring the repository for any commit activity.
  3. Create a `.travis.yml` file in the root of your GitHub repository defining the tasks you want Travis-CI to
perform each time a commit is made to your monitored repository.

### Add a `.travis.yml` File to Your Repository
Once you've enabled Travis-CI for your LaTeX repository, it's time to create that `.travis.yml` file which will tell
Travis-CI what automated tasks to perform each time a commit is pushed to your repository. First, a little background:

When Travis-CI detects a commit to your repository it first create a brand new, clean (vanilla) virtual machine (VM)
called the [build environment](http://docs.travis-ci.com/user/ci-environment/) (by default: Ubuntu 12.04 LTS Server
Edition 64bit). Next, Travis-CI clones your repository within that build environment and then runs the tasks defined
in the `.travis.yml` file in the top level of your repository. Now, what do we need to add to the `.travis.yml` file to have Travis-CI perform:

  1. Install TeX Live
  2. Compile our LaTeX document

License
=======
The contents of this repository are released under the Creative Commons
Attribution 3.0 Unported License. For a summary of what this means,
please see:
http://creativecommons.org/licenses/by/3.0/deed.en_GB

