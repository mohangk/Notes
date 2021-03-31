---
draft: true
title: Real World Devops
categories:
  - Talks
---
Intro

- Devops is not a job title 
  - it definitely involves hiring or developing folks who can cross both disciplines - but if you just have one department dong it, its not devops
- Typically organizations is Dev vs Ops - why? because focus on the immediate objective as opposed to overall objective?
- its getting people to work together across disciplines and across departments towards a common business goal
  - it means not getting in the way of each other, without sacrificing what is important to each  group
    - e.g. to dev teams - releasing features, ops - ensuring service stays up and not get paged 
- its enabling faster feedback for everyone in the process

And don’t EVER make the mistake that you can design something better than what you get from ruthless massively parallel trial-and-error with a feedback cycle. That’s giving your intelligence much too much credit.
       — Linus (http://tinyurl.com/2kkl77)
- avoiding memes like 
  - works in dev 
  - https://www.reddit.com/r/ProgrammerHumor/comments/2q86rb/people_in_it_xpost_from_rfunny/

Don’t 

1. Have a priesthood/gatekeepers - who can deploy into production
  - leverage tools not personnel  
2. Create silos
  - Full stack - stack extends to how things run, specialisation yes, but base knowledge everyone
  - Access to source code, Jira, PivotalTracker, SlackRooms
  - feature branches
    - gitflow for the most part 
3. Hire monkeys, this
4. Reporting into same org, with same value / principals / interests
   - moral hazards
     - dev & test , ops & dev
     - definition of “done”  => out into production - everyone owns that

Do:

1. Provide autonomy but within guardrails & responsibility
  - ops owns platform tooling, self service systems, conventions
    - Common configuration mgmt, orchestration tools   
  - dev teams own configuration
  - dev teams get paged on issues
    - allows faster as contextual overload if large systems is too complicated 
    - ensures that production scalability & stability is front and center for the team - as they don’t want to be paged - less animosity

	  -UNIX was not designed to stop its users from doing stupid things, as that would also stop them from doing clever things.
	        — Doug Gwyn

    
2. Cross train
  - pairing between dev / ops is critical
  - specialization is less critical - common ops practices	
  - facilitate information exchange - shared workstations, pairing desks, personal goals

3. Leverage managed services 
  
  - best ops is no ops
  
  - lock in vs time to market
  - adoption of open standards

  - Keep orchestration to an absolute minimal, leverage internal tools within cloud provider as much as possible - just more shit to maintain
  - simplify, then simplify some more
  - Simplicity is prerequisite for reliability.
        — Edsger W. Dijkstra

  - unknown issues