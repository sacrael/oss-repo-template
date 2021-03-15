#### Abdul-Muiz Yusuff
# Analysis of Open Source Projects


## Overview
In this analysis, I will be giving an overview of the analysis data 
colleded for 3 repositories:

1. Seed -> a front-end Rust framework for creating fast and reliable web applcations with an elm-like structure. https://github.com/seed-rs/seed
2. Flutter -> a mobile application SDK for building high-performance, high-feidelity apps for iOS and android from a single codebase. https://opensource.google/projects/flutter
3. Godot -> a feature-packed, cross-platform game engine to create 2D and 3D games from a unified interface. https://github.com/godotengine/godot

I picked these project from various interests in technologies and softwares that I would like to learn about. My growing interest in Rust programming language and Web Assembly led me to pick Seed as a project of interest. I chose Flutter for my second project to analyze because I am interested in the advancements the project is making in terms of desktop application development that could replace existing popular choices such as Electron and Qt. Finally, I chose Godot as my third project to analyze because I am fairly interesed in game engines and learning the processes used to develop such a software. Without further ado, let's get right into it!


#### Project #1
## Seed: Frontend Rust Framework using WebAssembly
Repository `https://github.com/seed-rs/seed`

| Evaluation Factor                  | Level (0-2)| Evaluation Data |
| -----------                        | -----------|------             |
| Licensing                          | 2          | The repository has an MIT License, which is approved by the OSI    |
| Language                           | 2          | 100% Rust     |
| Level of Activity                  | 2          | The repository is fairly active, with a decent amount of commites every month for the past year    |
| Number of Contributions            | 2          | There are 43 contributors     |
| Project Size                       | 2          | The project has 27.4k lines of code. This is a moderate size for a project I would be looking to contribute to.    |
| Issue Tracker                      | 2         | The issue tracker has fairly active issues being added and closed, along with a decent amount of discussion happening under each issue. Each issue has very descriptive tags describing what each issue is associated with.  There are 55 open issues and 207 closed issues. The third issue, `unhandled error with browser autofill`, was created on `December 14, 2020` |
| New Contributor                    | 1         | There are instructions on how to contribute to the project, but it is a fairly sparce guide. There are also instruction on how to install the download the development environment. Theere is a discord listed on the repository used for collaborator communication. There is also web presence for the project. They have a detailed guide on the use of the project at `https://seed-rs.org/`.   |
| Community Norms                    | 2          | There is a code of conduct that can be found in the root directory of the repository. There does not seem to be any rude or inappropriate behaviors from what I can see. The code of conduct contains a scope that describes the  vision for the project. Additionally, it lists the responsibilities for the contributors to ensure the success of the project. Lastly, they list how they enforce the code of conduct. Looking through the project's discord, there seems to be fairly nice and sophisticated communication going on. A bit of joking around, but nothing that would be a red flag. They help out newcomers on questions that they may have. Overall, good community, looking from the outside.  |
| User Base                          | 2          | There appears to be a small user base, mostly people experimenting with the possibilities. But that is a given when working on a WebAssembly project as it is still making its development rounds to become a stable framework. Instructions on how to get started can be found on their website, along with how to use the application, `https://seed-rs.org/`.      |
| Total Score                        | 17        |  |

#### Project #2
## Flutter: A Mobile Application SDK for building Cross-Platform Apps
Repository: `https://github.com/flutter/flutter`

| Evaluation Factor                  | Level (0-2)| Evaluation Data |
| -----------                        | -----------|------             |
| Licensing                          | 2          | Flutter has a BSD-3-Clause license, which is approved by the OSI     |
| Language                           | 2          | The languages they use are Dart (99%), Objective-C (0.2%), and Java (0.2%). They use 0.1% of C++, which is the language I am more intrested in working in. For a codebase that is 1.3 million lines, 0.1% is still a significant amount. Additionally, since I am interested in helping the development of the desktop application development, I would be working more with the C++ code and Dart, instead of Objective-C and Java.     |
| Level of Activity                  | 2          | There is very high level of activity in Flutter. For every month in theis year, there has been over 100 commits.    |
| Number of Contributions            | 0          | There are 803 contributors.    |
| Project Size                       | 1          | 1.3 million lines of code. This project is way too big. I may be able to contribute by focusing on a very small portion of the project, but understanding what is going on a codebase that is this large will be a little tricky.     |
| Issue Tracker                      | 2          | There is a very robust issuing system in place in the flutter repository. At the current moment, there are `8,380` open issues and `42,074` closed issues. The third issue on the issue tracker was opened 3 hours ago, `February 25th, 2021 at 6 PM EST`. From browsing the repository, issues are very actively being added and resoled.   |
| New Contributor                    | 2          | There are very clear instructions on how to install and download the development environment. There is an active discord server for communication that can be found on the repository. Discussions under issues are very active.     |
| Community Norms                    | 1          | The code of conduct is very sparse. They include conflict resolution and where to ask questions. The conflict resolution portion focuses on how to resolve disputes between contributors, so it seem they are not interested in tension wihin the community. Looking at the discord, there is a lot of communication going on since this is a fairly large project. It seems to be well moderated and no interaction seems to be toxic or unwelcoming.   |
| User Base                          | 2          | There is a very active user base that develops ready-to-ship applications deployed on iOS and Android mobile app stores. There is a growing web development sub-community as flutter's web deployment increases in compatibility.     |
| Total Score                        | 14         |  |

#### Project #3
## GODOT: A Feature-Packed, Cross-Platform Game Engine for 2D & 3D Games
Repository: `https://github.com/godotengine/godot`

| Evaluation Factor                  | Level (0-2)| Evaluation Data |
| -----------                        | -----------|------             |
| Licensing                          | 1         | The license does not seem to have a name, but allows the distribution and contribution of the software as is. It is worded in a way that it seems to be open source, but without a name to refer the license to, it is not approved by the OSI.    |
| Language                           | 2          | The top 3 languages used in GODOT are C++ (89.8%), C (2.4%) and C# (2.2%). These are the languages that I am interested in working in, more specifically C++, so it aligns with my interests in that regard.     |
| Level of Activity                  | 2          | There is a very high level of activity. Every month of this year has about 80+ commits, so the contributions are very active.     |
| Number of Contributions            | 2          | Three are 1,355 contributors, so there is a vast surplus of developers contributing to this project.     |
| Project Size                       | 1          | The project has `5.6 million` lines of code in this project. This is a lot of code to get accustomed to in order for a newcomer to contribute, but I would assume that not all of the code is necessary to understand in order to be able to get up and running for contributing, but it's a fair amount to get used to.     |
| Issue Tracker                      | 2          | The issue tracker is in amazing shape. There are `4,968` open issues and `21,649` closed issues. Additionally, the third issue on the issue tracker was opened 2 hours ago, `February 25th, 2021 at 8 PM`. So issues are being updated pretty frequently. Additionally, there is a fair amount of discussion going on for each issue, so the community is fairly hands on in terms of resolving active issues.     |
| New Contributor                    | 2          | There are very clear instructions explaining how to setup the development environment for new contributors. The communication method, just like the last two projects, is Discord. They have a discord server that is used, I presume, for discussing updates, new features, and issue resolution. The web presence is very high, as GODOT has is gaining increasing amounts of attention from the game development community. It is recommended as one of the game engines to start with when getting into game development, along with Unity Engine and Unreal Engine.     |
| Community Norms                    | 1          | There does not seem to be a code of conduct acessible from the repository. The discord seems fairly tame. Nothing being said that is unpleasant to newcomers, and a decent amount of discord moderators hopefully managing the discourse on the discord channel.    |
| User Base                          | 2          | Godot has a large user base and is growing each year. I heard about the game engine making rounds about 3 years ago and now it is starting to staple itself as a very popular game engine. They fundraised `$15 million` this year to its continued development.     |
| Total Score                        | 15          |  |


## Diving Deeper
### Seed - A Frontend Rust Framework

From the three projects I chose to analyse, I am most interested in the Seed project in terms of contributing. From the three projects, it scored the highest on the rubric . I gave Flutter and Godot a 1 score on the project size and community norms. Both Flutter and Godot are massize open source projects. I would say they have an overabundance of developers, when comparing their number of contributors to the amount of commits per month. So, I am less inclined to pick these projects because my contributions would be less impactful and the amount of code I would need to read through to be at  a state where I understand what is going on in the codebase is too much. Flutter is at `1.5 million` lines of code, whereas Godot is at `5.6 million` lines of code. Seed is at a reasonable amount of `27.4 thousand` lines of code. Additionally, of the three projects, Seed has the more in depth and welcoming code of conduct. So, just from looking at the total score, I would pick Seed over Flutter or Godot.

In terms on what I would get out of contributing, I am also more inclined to contribute to Seed because I will be walking away learning more from that experience that contributing to Flutter or Godot. The technology used in the Seed project is Rust programming language and WebAssembly. I would be able to walk away with more knowledge in both of these areas, while also contributing to the early stage development that WebAssembly is in. Whereas in Flutter and Godot, I would just be adding practice to my C++ programming experinece, which is good, but I can find that experience very easily. 

Lastly, due to the smaller size that the Seed project has in terms of a development community, it would be easier for me to get accustomed to the developers in the community and start a conversation, versus the monolith that is Flutter and Godot, where there are about `1000` contributors on the project. It's much harder in this project to get a glimpse of who's doing what and how many developers are working on different parts of the project.

So it seems that Seed is the winner.