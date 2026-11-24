# StepBuddy

A patient, step-by-step tech helper for parents 

Contents: Overview · Personas · Goals · Use cases · Use case diagram · User flow · Functional requirements · Non-functional requirements · Architecture · Sequence diagram · Tech stack · Risks · Success metrics · Roadmap

Overview and problem
StepBuddy is a free web app that walks parents through any confusing phone or computer screen, one step at a time, in their own language.

The problem. Older parents get stuck on everyday tasks: joining a video call, changing text size, connecting to Wi-Fi, finding a photo. Today they phone a busy son or daughter, who has to guess what the screen looks like. Help arrives late or not at all, and the parent feels like a burden. Scam screens (fake prizes, OTP requests, remote-access apps) catch them most easily of all.

The solution. The parent sends a screenshot and asks by voice or text. An AI agent reads the screen, checks it for scam signs, and shows the next single step with a pulsing circle on their own screenshot, read aloud. After each step it asks for a new screenshot and checks they got there before moving on. That look, act, verify loop is what makes it an agent rather than a chatbot.

Target users and personas
The parent is the only person who uses the app day to day; the grown-up child sets it up once.

Persona	Who they are	Needs	Frustrations
Amma, 62 (primary)	Retired teacher, owns an Android phone, prefers speaking Tamil, uses WhatsApp and YouTube daily	Clear, single steps; big text; voice instead of typing; reassurance	Small buttons, English-only menus, feeling she is bothering her children
Appa, 66 (primary)	Retired, uses a laptop and a phone, mixes English and his mother tongue	Help with forms, payments, and settings without being rushed	Jargon, too many steps at once, fear of pressing the wrong thing or being scammed
The grown-up child (secondary)	Works full-time, often in another city	Set the app up once, trust it is safe, keep strangers from using the free quota	Long phone calls guessing what the parent sees
The personas are illustrative; replace them with notes from watching your own parents use their devices.

Goals and non-goals
Version 1 succeeds if a parent can finish a common task alone, guided only by StepBuddy, at zero running cost.

Goals (v1)

A parent completes a common task (video call, text size, Wi-Fi) without calling anyone.
Every new screenshot is checked for scam signs before any step is given.
Replies come in the language the parent spoke or typed, both on screen and read aloud.
Works in any modern browser on phone or computer, with nothing to install.
Costs nothing to run: free AI tier, free hosting, no paid services.
Non-goals (v1)

Controlling the parent's device remotely or tapping for them.
Live screen sharing or video; v1 works from still screenshots only.
User accounts, cloud history, or a dashboard for the child.
Native iOS or Android apps.
Translating the app's own buttons into every language (the AI's replies are translated; the interface stays English with icons).
Use cases
Ten use cases cover v1. The parent drives all but one, and the Gemini AI is the secondary actor in every guided step.

ID	Use case	Actor	Trigger	Main flow	Outcome
UC-01	Get step-by-step help	Parent	Stuck on a screen	Adds a screenshot, asks a question, taps Help me; the agent runs UC-02 and UC-03	First step shown and read aloud
UC-02	Check screen for scams	Gemini AI (included in UC-01, UC-04)	Every new screenshot	Agent rates risk as none, caution or scam, with a reason	Red "Stop" card on a scam; the step steers to safety
UC-03	Show next step on screenshot	Gemini AI (included in UC-01, UC-04)	Agent knows the goal	Agent returns one instruction plus a box around the target	Pulsing circle on the parent's own screenshot
UC-04	Verify progress	Parent	Finished a step	Taps "I did it", adds a new screenshot; agent checks it matches the expected screen	Next step, a correction, or the finish screen
UC-05	Ask by voice	Parent (extends UC-01)	Prefers speaking	Taps the mic, speaks in any language, taps stop	Agent detects the language and replies in it
UC-06	Answer a clarifying question	Parent	Agent unsure what they want	Taps one of 2 or 3 big option buttons	Agent continues with the clarified goal
UC-07	Flag a problem	Parent (extends UC-03)	Circle in the wrong place, or step unclear	Taps "Wrong spot" or "I'm confused"	Agent re-points or re-explains more simply
UC-08	Hear it again	Parent	Missed the spoken instruction	Taps "Say it again"	Instruction re-read in the reply language
UC-09	Learn to take a screenshot	Parent	Doesn't know how	Opens "How to screenshot"	Button combination for their device
UC-10	Set up and lock the app	Family admin (the grown-up child)	First deployment	Sets the API key and family code on Vercel, shares the link and code	Only family can use the free quota
Use case diagram
The parent reaches every guided use case; the safety check and the step pointer are included in every guided turn.

StepBuddy system
include
include
include
include
extend
extend
Actor: Parent
Actor: Family admin
Actor: Gemini AI
UC-01 Get step-by-stephelp
UC-02 Check screen forscams
UC-03 Show next step onscreenshot
UC-04 Verify progress
UC-05 Ask by voice
UC-06 Answer a clarifyingquestion
UC-07 Flag a problem
UC-08 Hear it again
UC-09 Learn to take ascreenshot
UC-10 Set up and lock theapp
Solid lines connect actors to use cases; dotted lines mark include and extend relations.

User flow
Every path ends in either the next step, a safe exit from a scam, or the finish screen.

yes
no
scam
none or caution
no
yes
I did it
Wrong spot or confused
Say it again
goal reached
Open StepBuddy
Family code needed?
Enter family code once
Home: add screenshot,speak or type
Tap Help me
Agent: safety check
Red Stop card, step to getsafe
Agent: identify screen andgoal
Clear what they need?
Clarifying question, bigoptions
Parent taps an option
Step card and pulsing circle,read aloud
Parent chooses
Add new screenshot
Agent re-points orre-explains
You did it: finish screen
The "I did it" path loops back through the safety check, so every new screenshot is checked before the next step.

Functional requirements
Fifteen requirements; the eleven marked Must are the v1 release bar.

ID	Requirement	Use case	Priority
FR-01	Parent can add a screenshot from the photo gallery or file picker	UC-01	Must
FR-02	Screenshots are shrunk on the device to about 1280 px JPEG before upload	UC-01	Must
FR-03	Parent can record a voice question; audio is converted to WAV in the browser	UC-05	Must
FR-04	Parent can type a question as an alternative to voice	UC-01	Must
FR-05	Agent calls the safety check first on every new screenshot	UC-02	Must
FR-06	A scam verdict shows a full-width red warning and is read aloud	UC-02	Must
FR-07	Agent gives exactly one step per turn, with a box around the target when it is on screen	UC-03	Must
FR-08	The screenshot shows a pulsing ring at the box and dims the rest	UC-03	Must
FR-09	Each new reply is read aloud automatically in the reply language	UC-03, UC-08	Must
FR-10	"I did it" opens the picker and sends the new screenshot for verification	UC-04	Must
FR-11	"Wrong spot" and "I'm confused" send a correction to the agent	UC-07	Must
FR-12	Clarifying questions show as 2 or 3 large tap-able options	UC-06	Should
FR-13	Static "How to screenshot" page for Android, iPhone, Windows, Mac	UC-09	Should
FR-14	Optional family code locks every page and the API behind a cookie	UC-10	Should
FR-15	Installable as a home-screen app (web manifest and icon)	UC-01	Could
Non-functional requirements
Accessibility comes first, because the users are older and often have weaker eyesight.

Area	Requirement
Accessibility	Base text at least 20 px; buttons at least 56 px tall; at most 3 choices per screen; WCAG AA contrast; respects reduced-motion; Atkinson Hyperlegible font
Simplicity	A first-time parent reaches their first step in 3 taps or fewer: add screenshot, speak, Help me
Performance	First step appears within 10 s of tapping Help me on 4G; the agent's progress notes show within 2 s
Request size	Each request stays under Vercel's 4.5 MB limit: only the latest screenshot and the latest voice clip are sent; voice is capped at 45 s
Privacy	No server-side storage; no accounts; a one-time notice not to send bank, password or OTP screens; free-tier AI inputs may be used by Google to improve its products
Safety	The agent never asks for or repeats passwords, OTPs, PINs or card numbers, and never guides a parent through a risky action
Cost	Zero running cost: Gemini free tier, Vercel Hobby, no paid APIs
Compatibility	Latest Chrome, Safari, Edge and Firefox on Android, iOS, Windows and macOS; the mic needs HTTPS
Reliability	Free-tier rate-limit errors show a friendly "try again in a minute" message and a Try again button
System architecture
Three parts: the parent's browser does the image, voice and speech work; one Vercel route runs the agent; Gemini does the thinking.

Vercel Hobby, free
Parent's browser, phone or computer
POST messages with latestscreenshot and voice
system prompt, messages,tool schemas
tool calls: checkSafety,identifyScreen, showStep
streamed message parts
Next.js React UI
Canvas: shrink screenshot to1280 px
MediaRecorder: voice toWAV
speechSynthesis: read aloud
Middleware: family codecheck
API route /api/helpAI SDK streamText + 5 tools
Gemini Flash APIfree tier
Nothing is stored on the server: the conversation lives in the browser tab and is sent with each request.

Sequence diagram: the agent loop
Each turn is one look, act, verify cycle, repeated until the agent calls finish.

Gemini Flash
API route on Vercel
Browser UI
Gemini Flash
API route on Vercel
Browser UI
loop
[Until the agent calls finish]
Parent
Add screenshot, speak question
Shrink image, convert voice to WAV
POST messages with latest screenshot and voice
System prompt, messages, tool schemas
checkSafety (risk, reason)
identifyScreen (app, goal, language)
showStep (instruction, box)
Stream tool calls as they arrive
Circle on screenshot, step read aloud
Does the step, takes a new screenshot
Taps I did it, adds screenshot
POST with the new screenshot
Messages and tools
checkSafety, then showStep or finish
Stream tool calls
Confirms progress, shows next step or You did it
Parent
The route stops each turn as soon as the agent calls showStep, askClarifyingQuestion or finish, which keeps each turn to one or two Gemini requests.

Tech stack
Every piece is free; development happens on macOS and the app is hosted on Vercel.

Layer	Choice	Cost	Why
Framework	Next.js 15 (App Router) + TypeScript	Free	Front end and API route in one project; deploys to Vercel with no setup
Styling	Tailwind CSS 4 + Framer Motion	Free	Fast, consistent large-type UI and gentle animations
Agent loop	Vercel AI SDK 5 (ai, @ai-sdk/react, @ai-sdk/google)	Free	Tool calling, streaming, and a step limit out of the box
AI model	Gemini Flash, free tier (gemini-flash-latest)	Free with rate limits	Reads images, returns button boxes, understands audio, writes many languages
Voice in	Browser MediaRecorder, converted to WAV	Free	Sends real audio so Gemini can detect the language
Voice out	Browser speechSynthesis	Free	Reads replies aloud using the device's voices
Validation	Zod	Free	Typed tool inputs the model must follow
Hosting	Vercel Hobby	Free (personal, non-commercial use)	HTTPS out of the box, which the mic needs
Code	GitHub	Free	Vercel deploys on every push
Dev machine	macOS with Node LTS and git via Homebrew	Free	The developer's own setup
Risks and mitigations
The biggest risk is the circle landing on the wrong button; the "Wrong spot" loop and the written instruction cover it.

Risk	Impact	Mitigation
Circle lands on the wrong button	Parent taps the wrong thing and loses trust	Instruction also names the button's colour, label and position; "Wrong spot" button asks the agent to re-point
Free-tier rate limits or quota changes	App stops answering at busy times	Friendly retry message; family code keeps strangers out; model id is an env var, and Groq's free tier is a one-line swap
Sensitive data in screenshots	Bank or OTP details reach the AI provider	One-time notice, safety check flags it, nothing stored on the server
Scam missed by the agent	Parent follows a scammer's instructions	Safety check runs first on every screenshot with explicit scam patterns; agent never guides risky actions
No voice for the parent's language on the device	Reply is silent	Text is always shown; parent can install the language voice in device settings
Too many steps or jargon	Parent gives up	One step per turn, short sentences, tested with real parents
Request too large on long sessions	Upload fails	Only the latest screenshot and voice clip are sent; voice capped at 45 s
Success metrics
The headline metric is tasks finished without a phone call; measure it by watching your parents try 10 real tasks.

Metric	Target for v1	How to measure
Tasks finished without calling family	8 of 10 test tasks	Observed sessions with your parents
Circle on the right button	9 of 10 steps	Count "Wrong spot" taps per step
Scam screenshots caught	5 of 5 test scams	Fake prize, OTP, KYC, remote-app and collect-request screenshots
Reply in the right language	10 of 10 voice questions	Test in each language your family speaks
Time to first step	Under 10 s	Stopwatch on 4G
Running cost	0 per month	Vercel and Google AI Studio dashboards
Milestones and roadmap
Six milestones take v1 from PRD to a live link your parents use; dates are open until you set them.

 M0 PRD and diagrams.
 M1 Agent core. API route with the five tools; tested with one screenshot from the terminal.
 M2 Help screen. Screenshot picker, step card, pulsing circle, safety card, clarify buttons.
 M3 Voice. Record voice in any language and read replies aloud.
 M4 Verify loop and polish. "I did it", "Wrong spot", finish screen, screenshot guide, family code, home-screen icon.
 M5 Launch to family. Deploy on Vercel, test on your parents' real devices, fix where they hesitate.
Later (v2): a weekly summary sent to the child, buttons translated into the parent's language, an Android Share-menu shortcut, and saved "how I did it" guides to replay.


