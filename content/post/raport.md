+++
title = 'Raport'
date = 2025-12-16T13:56:04+02:00
draft = true
+++

#### (Page not indexed in the website, kept as draft.)


### Requirements

- Alert implementation and use cases suggestions.
- Improvement suggestions for the 2 platforms, CyberQuest/NetAlert.
- Improvement suggestions for the documentation.
- Bug report in case of findings.

### Alerts

I`ve created a customized alert whose purpose would be to catch in the span of an hour, any attempt to create a new user on a linux machine, establish a session connection to that respective user and try to gain super user privileges on that newly created user while outside of the working hours. This alert has an associated action that is supposed to gain the UID of the newly created user from one of the conditions and pass it into a user block.

**Screenshots of the config:**

<img src="/img/Raport/rule1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">


<img src="/img/Raport/rule2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">


<img src="/img/Raport/rule3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">


<img src="/img/Raport/action.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

#### Flow to produce the alert using the provided ubuntu machine

**admin terminal:**
```
superadmin@Ubuntu-Practica:~$ sudo useradd testalert
superadmin@Ubuntu-Practica:~$ sudo passwd testalert
New password:
Retype new password:
passwd: password updated successfully
```

**new user terminal:**
```
$ su
Password:
su: Authentication failure
```

#### Other use cases?

- Blocking a user after trying to access a resource owned by a group he is not part of.
- Alerting on mass user creation.
- Alerting when a user gets put in a group he was not previously part of.

## Bugs?

#### 1. Inability to properly use space for values

When interacting with a textbox in the alert creation menu, typing a value (no matter its type, string or int) doesn`t allow you to insert whitespaces after the last element. This can only be achieved by inserting them in between the first and last character of your desired value.

<img src="/img/Raport/value1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

<img src="/img/Raport/value2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

#### 2. Bug when trying to delete an alert step in case of a long description

If the title description of an alert is longer than usual, the text is rendered on top of the delete button and it can`t be pressed unless you reload the page in order for the alert tab to not get selected in the first place.

<img src="/img/Raport/bugbuton.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

### Improvement suggestions

**Being able to move rules (maybe through drag and drop)**

Creating an alert rule that should be referenced by an earlier alert doesn`t work, you would have to remove them and make them again. A reordering system for those rules might come in handy (Have not found one, do not know if there is one).

<img src="/img/Raport/imp1.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

**Save button**

A save button addition instead of only having a save and exit button might come in handy, in case you want to save some progress and not directly quit.

**The ability to duplicate conditions**

A button for duplicating your current condition row just like the one for alerts could save some time in case someone has to do a lot of repetitive conditions one after the other.

### Relevant improvements to the documentation

- Group content by user persona. Many users will have distinct goals (setup vs API vs investigations).
- Add step‑by‑step use cases for common workflows (e.g., configuring alert forwarding, building a correlation rule).
- Include working CLI/API examples, JSON, or sample scripts with inputs/outputs for relevant API sections.
- Offer topic filters on search results (by category like API, UI, Alerts, etc.).
<br></br>