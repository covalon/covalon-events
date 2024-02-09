# Covalon Events Module
This module contains compendiums for all Covalon Event related resources like journals, soundtracks, maps, and so on.

## Getting Started
Foundry sucks and made everything way more complicated so you're gonna need something called Git. Luckily, Github has made a program to help make this easier!! Go download https://desktop.github.com/ and login with the Github account you're gonna use for Covalon.

Clone a repository from the internet, go to the URL tab, and paste in `https://github.com/covalon/covalon-events`. Set the local path to your Foundry modules folder (looks something like `C:/Users/yourusernamehere/AppData/Local/FoundryVTT/Data/Modules`) - it _SHOULD_ automatically append a /covalon-event at the end of the local path when you've properly selected it, but if it doesn't, add it. Then hit the clone button.

When it's done, you should now have a folder called **covalon-events** in your modules folder, and have a window saying `no local changes`. You will want to open up Github Desktop whenever you're going to edit the module.

When you are making content edits, please work on a local copy of Foundry so you can easily access your files. Make a world specifically for working on the Covalon modules - it should be the only one active on that world. (I do not think there is any harm in running Covalon and Covalon Event module in the same world but I have no guarantees - I think file changes will happen even just opening Foundry, so making another world probably won't help.)

If you are ever uploading images, please make sure to upload them into `modules/covalon-events/images`

## DO THIS BEFORE YOU MAKE ANY CHANGES - EVEN BEFORE YOU OPEN YOUR FOUNDRY!!!

Due to the nature of the databases, we should only ever have one person uploading changes at a time. That is to say if multiple people are creating actors or items, they should export the data and send it to someone to add to the compendium and upload to Github. It's still fine to have multiple people upload changes, but for example, if I'm working on something and Arto is working on something at the same time, we're going to run into issues when we try to upload.

- With Foundry closed, open up Github Desktop. It should say No Local Changes. If it does, hit the button called `fetch origin`.
  - If you *have* accidentally opened your Foundry world and find there's several changed files, go to Branch > Discard all changes, and confirm. This should reset things. *Then* hit the `fetch origin` button.

- When it's done, proceed with the steps listed in the section relevant to you. This will usually be either **Editing existing compendiums** OR **Adding new compendiums**, and then followed up by **Finishing up and making a new release**.

- PS, you can use the `show in explorer` button to be linked straight to the module folder!

## Editing existing compendiums
This is as simple as going into Foundry after you've pulled down the latest changes, and making your edits as needed.

## Adding new compendiums
Unlike adding new content to existing compendiums, creating new compendiums is a little harder, especially with our structure. These steps are required to properly maintain folder structure - V11 is currently very finnicky >:(

The following instructions assume you are creating new expeditions:

- Open the `module.json` and make the following edits:

  - Immediately after the `packs` array starts, **between lines 26 and 27**, insert the following code, replacing all `location` with the appropriate name. Note the capitaliation.
  ```json
    {
      "label": "Event Actors",
      "name": "event-actors",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "path": "packs/event-actors",
      "type": "Actor",
      "system": "pf2e",
      "ownership": {
        "PLAYER": "NONE",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Event Journals",
      "name": "event-journals",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "path": "packs/event-journals",
      "type": "JournalEntry",
      "ownership": {
        "PLAYER": "NONE",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Event Scenes",
      "name": "event-scenes",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "path": "packs/event-scenes",
      "type": "Scene",
      "system": "pf2e",
      "ownership": {
        "PLAYER": "NONE",
        "ASSISTANT": "OWNER"
      }
    },
    ```
  If you end up needing items other than the standard actor / journal / scene, you can copy and paste one of the blocks and replace all mentions of Actor to i.e. Item, noting capitalization. See (https://gitlab.com/encounterlibrary/my-content-module/-/blob/main/module.json) for how to format the `type` field.

  - Find the `packFolders` array. Unfortunately, I cannot give you a line number, but it should be near the bottom of the json. Within it, find the section labeled like: `"name": "Covalon Events"` and, after the `folders` array starts, add the following code between that line and the next, replacing all `event` with the appropriate name. Note the capitaliation:
  ```json
  { "name": "Event", "sorting": "a", "packs": ["event-actors", "event-journals", "event-scenes"] },
  ```
  If you added any extra packs in the step above that need to go into the folder, add their `name` into the `packs` array.

- Save the `module.json` and then go and open Foundry again. If you have the Covalon Events module activated in the world, you should now see this new folder and its Compendiums.

- Unlock the relevant compendiums and add your content!

## Finishing up and making a new release
**MAKE SURE FOUNDRY IS CLOSED BEFORE YOU PROCEED.**

In your local `module.json`:

- Update the version number, using the format `major.minor.patch`, found on **line 8**

  - If you only had to edit existing compendums, update the **patch version**. (`1.0.4 -> 1.0.5`)

  - If you had to add a new compendium, update the **minor version** and reset **patch** to 0. (`1.0.5 -> 1.1.0`)

  - If Foundry or PF2E gets a major update (i.e. v10 to v11) that completely breaks the module and we have to remake things in their new versions, update the **major version** and reset **minor** and **patch** to 0. (`1.1.6 -> 2.0.0`)

    - Additionally, update the minimum compatibility the Foundry/PF2E version you're on. If Foundry broke things, change `minimum` on **line 10** to the Foundry version (`10 -> 11`). If PF2E broke things, change the `minimum` on **line 20** to the PF2E version (`4.0.0 -> 5.1.0`). If both of them broke things, change both.

Time to upload everything! Open up **Github Desktop**. You should see it say something like `160+ changed files`. It's a lot, I know.

- In the `Summary (required)` field, type in the version number you wrote into module.json, prepending it with a v, like so: `v2.0.5`

- Due to the new Foundry format, it's much harder at a glance to see what content has changed. I like to use the `description` field to wrhte my changelogs, and then copy and paste that when I do my release too. This is optional.

- Hit the `Commit to main` button!

- When that's done, hit the `Push origin` button.

- And when *that's* done, it's time to hit the `View on Github` button. You should see, once it's loaded, next to the README.md and packs folder, the text you put in `summary` i.e. `v2.0.5`

- Hit the `Create a new release` button
  - Choose a tag (set the name with the name used in your Summary, i.e. `v2.0.5` and then create new tag)
  - Make the `Release title` your version number, same as the tag
  - Try to accurately describe every change you made in the `Description` field, as the new Foundry update makes it a lot harder to accurately track our changes
  - Once you're ready, `Publish your release`!

Hopefully this should cover everything...?
