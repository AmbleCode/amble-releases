# Build together in Amble

Amble runs your projects and agents on a computer you own. This guide covers working
with other people and other computers: who can reach what, how to follow work already in
progress, how to divide work between agents, and how to share an app with testers.

Controls depend on the installed host and client versions. Start with the
[installation guide](INSTALL.md) and check the
[release notes](https://github.com/AmbleCode/amble-releases/releases) when a
control described here is unavailable.

## Choose the right access

The computer running a project is its **host**. Each host approves its own
connections. Being on the same network does not grant access, and pairing a device does
not grant Owner access unless you choose it.

| Access | Who it is for | What it allows |
| --- | --- | --- |
| **Owner** | Your own devices, or someone you would trust with your computer | Everything on the host: projects, agents, settings, other devices and the whole desktop. |
| **Viewer** (shown as **User** when you invite someone) | A tester or client | Use of the apps you explicitly share. No source, agent conversations, terminals, host settings or computer screens. |

Agent commands run with the host account's operating system permissions, so an Owner
device can do anything that account can. Give Owner access only to people and devices you
trust that fully. Viewer access never includes agents, source files or the desktop.

Earlier versions offered a **Developer** level for selected projects. Hosts now read an old
Developer grant as Viewer. To work on the same project as a teammate, either pair their
device as an Owner (only if you would trust them with your computer), or have each person
run their own host and share changes through Git.

### Invite a device

1. On the host, open **Pair device** and choose **Add a device**.
2. Pick the access level, **Owner** or **User** (Viewer), and name the device or person.
3. Create the invitation. Show the QR code, or choose **Copy pairing link** and send it privately.
4. On the other device, choose **Add connection**, then scan the QR code or paste the link.
   On the same network, it can instead pick this computer from the nearby list and type the
   six-digit code the computer shows.

An invitation works once and must be accepted within five minutes. That window is the time
to accept, not the lifetime of the access it grants; access lasts until you remove it. Open
**Pair device** (or **Manage devices and apps** in the Share app window) to review and remove
devices. Pairing codes and links let a device connect, so keep them out of screenshots and chats.

## See the work already in progress

An Owner device can open the project's workspace and agent conversation to read the request,
the agent's response and the work it has already attempted. Choose the same host and
conversation to follow the same session; a separate chat has its own context.

When several devices queue requests for an agent, the shared pending queue shows the
originating device label. Check those requests before sending another task so you can spot
work that is already waiting. Queue controls and attachment support vary by host version. A
device label is display information, not verified personal authorship for every message.

Some conversations have narrower access. In particular, a remote model conversation can be
private to the person who opened it. Project access does not override that restriction.

### Respond to an agent's work

Continue a conversation with a question, correction or next step. For a specific response,
use its copy control and include the relevant passage in your message. Sending feedback to an
agent can start more work; it is not a passive note visible only to teammates.

For code feedback, the **Changes** view supports inline review comments on diff lines. On
connections with that review workflow, add your comments, return to the composer and check
the review attachment before sending. Review drafts stay on the device until submitted. An
older host or client may not support the full review flow; use a text follow-up instead.

These are conversation follow-ups and code review comments. There is no separate team
comment thread attached to every AI message.

## Split the work before starting agents

Give each task an outcome, a responsible person or agent, an area to change and a check that
will show it is finished. For example:

| Task | Responsibility | Review evidence |
| --- | --- | --- |
| Improve the checkout layout | One person and a UI agent | Try the page at desktop and phone sizes. |
| Add checkout validation tests | Another person and a testing agent | Review the tests and their results. |
| Integrate the changes | The person responsible for the final branch | Review both diffs and test the combined app. |

Use separate agent conversations for independent tasks. Give each agent the context it needs;
opening another conversation does not automatically copy all other conversations into its
prompt. Review existing conversations and pending requests before assigning more work. Amble
makes work visible, but task ownership still needs to be agreed by the team.

The optional **Amble Teams** add-on, installed from Plugins, can help: it organizes existing
conversations into assignments with an owner, acceptance checks and dependencies. It starts no
agent on its own and creates no new computer permissions.

### Choose a shared workspace or separate worktrees

In one workspace, people and agents use the same checkout. This is useful for reviewing a
shared result or handling carefully separated changes. It also means overlapping edits can
affect each other immediately.

For independent code changes, create a **New workspace** with **New worktree** isolation, or use
the project's **New worktree** action where available. A Git worktree gives a task a separate
checkout and branch. It needs its own applicable setup, dependencies and preview configuration.

Worktrees separate uncommitted edits. The team still reviews, merges and resolves conflicts
when bringing branches together. A separate conversation alone does not create an isolated
checkout, and connecting another computer does not automatically synchronize its repository
with the first one.

## Coordinate agents across computers

You do not have to choose a mode. Amble agents decide for themselves whether to work alone,
start helper agents or use another computer, so you can ask in plain words, for example
"run the Windows tests on the Windows computer". An agent asks before its first task on
another computer and can remember your answer for the project. Each target computer keeps its
own permissions and must be paired and reachable.

For explicit control, type **/orchestrate** in the composer. The sheet asks **How should this
run?**

- **On this computer:** this chat's agent starts helpers here, optionally on helper models you choose.
- **Across computers:** one computer plans and others do parts. It needs another paired computer.
- **Remote agent here:** a model on another computer works on this project.

For **Across computers**, choose how to split the task. **Coordinator plans** lets a coordinator
computer divide the job into different assignments; review the plan and check which workspace
each worker will use before starting it. **Same task everywhere** sends the same task to every
selected computer, which suits independent comparisons rather than dividing a job. Selecting
several models on one computer can also give each model the same assignment, so check for
overlapping writes before dispatch. Opening the sheet never sends your draft.

Inspect the per-worker results after dispatch, including any partial failures. Agents already
started have their own lifecycles; closing the sheet does not stop them. Scripts can delegate to
a connected computer from a terminal with `amble host-agent`.

Cloud and local models can take different responsibilities. Choose from the models actually
configured and supported by each agent and host. A powerful computer can run a supported local
model while you direct its work from another device. Hardware, model capability and host
availability determine what that setup can do.

## Validate together and invite testers

Have someone other than the task's implementer review the changed files and test results. The
host owner can use **Changes**, the project's terminal and Git workflow to inspect and integrate
the result. Test the combined branch as well as each individual change.

Use **Open app** to try the actual running app. Check the important actions on the devices
people will use, then bring specific feedback into the agent conversation. A completed agent
turn or a running process is not proof that a test passed.

Use **Share app** for someone who only needs to try the app. It shows the app it will share and
marks the invitation **App-only access**, so source files, agent conversations, terminals and
settings stay private. You can remove that access at any time.

The host must stay awake, connected and running Amble. Previews on your local network are free.
Reaching a preview or the desktop from outside your network needs Premium or above, and monthly
remote viewing hours depend on your plan. See [plans](https://www.amblecode.com/#pricing) and
[connection guidance](https://www.amblecode.com/docs/pairing.html).

## When something is missing

| What you see | What to check |
| --- | --- |
| A teammate cannot see a project or conversation | Confirm the selected host and the access level you gave their device. A Viewer sees only shared apps. |
| A worktree has different files or an older result | Check its branch, commits and preview command. Merge the intended changes before testing the combined result. |
| A queue or review action is unavailable | Read the displayed reason and check both host and client versions. Preserve the draft before changing connections. |
| Two agents are changing the same files | Agree on one writer, stop or redirect the overlapping task, or move independent work into separate worktrees. |
| A phone cannot connect or open the preview | Check the host's power, network, running app, the device's access and, outside your network, your plan. |

[Back to Amble](README.md) ·
[Report a problem](https://github.com/AmbleCode/amble-releases/issues/new?template=bug_report.yml)
