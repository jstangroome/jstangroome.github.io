---
layout: post
permalink: http://blog.stangroome.com/?p=53
title: Staying Optimistic
description: None
date: 2007-09-20 22:09:39 -0000
last_modified_at: 2007-09-20 22:09:39 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
The topic of concurrency control in database applications has been on my mind lately and I wanted to get my thoughts in order. Here I intend to present my views on the situation and probably contort some official definitions in the process.

At the outset there are two choices: pessimistic concurrency that will lock data from the time an edit begins until the time the edit is committed or optimistic concurrency that does not lock but only commits data that has not been changed by another process since the edit began.

Pessimistic concurrency has it's place but for user databases it is losing popularity for two reasons. Firstly it requires additional server resources to track which data is locked and by whom, and secondly it means that one user will need to wait before working on the same data that another user is editing.

Optimistic concurrency avoids the first issue by having the client remember a checksum or timestamp of the original data that it can check for at commit time to ensure no other changes have been made. The compromise is that if changes have been made, the user has to either re-input their changes to the new data or use a merging tool (if available) to reconcile the differences.

Obviously, optimistic concurrency is going to work really well in situations where it is rare for two users to edit the same data and in practice with databases full of thousands of records and a only a relatively small group of users working with them, the chances are low. (So low that many database applications don't bother with a merging tool).

However, time is a major factor with optimistic concurrency. The less time between the reading of the original data and the final committing of new data the less likely another user has changed the same data. If a user begins updating a customer's address though, but stops to answer a phone call then go to lunch, when this user returns and remembers to save there is a much greater chance a fellow employee has worked on the customer record.

There are additional factors beyond a user's actions that can increase the chance of an optimistic concurrency violation. It is fair to expect that an application should load at least a subset of records when it first starts to give the user a populated initial view but now the clock is ticking because the application has remembered the state of the data at the time the program opened. Assuming the architecture handles it, this is easily resolved by re-retrieving the latest data from the server immediately before the user begins making changes.

Also, while it may only take 30 seconds for the user to type in a new phone number for an existing customer, the chance of a concurrency violation continues to grow as each minute passes until the application pushes these changes back to the database. In this example having the application save the new data to the server when the user closes the customer dialog is an obvious solution. But time between read and commit is not the only concern...

When there is a concurrency violation, some action needs to be taken. An easy but often inappropriate action is to simply ignore the changes on the server and overwrite them with the latest data from the user. If that was acceptable for your application you wouldn't be here worrying about concurrency. As mentioned briefly above the options are to compare and merge or discard and re-input.

With either approach, the difficulty to merge or re-input grows proportionally with both the time since the edit was made and the amount of data being committed. The difficulty lies not only in the user's ability to remember all the changes they have entered but also the application's ability to determine the concurrency relationships between the various data elements being committed (ie if an existing order conflicts due to a changed shipping address should the added order line be considered a conflict too?).

Use optimistic concurrency, save often, save automatically.
