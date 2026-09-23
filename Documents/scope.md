# General Scope

## First Thoughts:
    - The project will focus on create a system to manage RPG character sheet, starting from creation to edition.
    - It must follow some rules, for example, not be too arbitrary and give freedom to the user.
    - The ideal state of this software will be when the user can manage to import/create any RPG System, create and 
    create sheets for either a character or npc.

## The Initial Idea:
    - The system will be produced with a schema of tags and classes, providing the user not a pre produced list of 
    systems, but in reality just options and screens to transfers one's ideas into a system to automatize progression 
    and actions.
    - For example, by don't disponibilize a straightfoward attributes system, it gives the user a way to create a tag 
    "Atribute", inputing it math, as "Atribute = Input / 2", so creating another tag "Strengh" amd giving it the 
    "Attribute", any input of Strengh (int or decimal), gives another two values, either Strengh by itself, and 
    Strengh.Mod, that it is the value divided by two.
    - Structuring like that create possibilities to adapt any system and idea easier to the program and either making 
    it easier to manage, fix and improve.
    - To do that, the prokect must use not a specific database (maybe use only temporary ones), but specially JSON 
    files, wich the import and creation will bring a new folder, separating the files on "{system name}.structure.json" 
    and "{system name}.{character name}.character.json", each one will be imported by the system in it own time and 
    says how the screen must be setted.

## Initial Structure:
    - It will be made in C# with Avalonia Framkework for UI, born to be a desktop app, althought when in a certain 
    point can have a mobile port.
    - As it will be my first project I may not choose the best arquitecture, but will try to use the NVVM one.