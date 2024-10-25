# 100 Days Of Code - Log

## Day 1: October 24, 2024

#### Today's Progress

1. Created my GitHub Profile page.
1. Revisited building prototypes using Xcode's Interface Builder and faking implementation
   details using animations.

#### Thoughts on GitHub Profile pages

I enjoyed creating my GitHub Profile page. Writing copy takes way more brain power than I
expected it would—just thinking about word choice and the different tones they evoke.

#### Thoughts on Prototyping

I’m currently implementing a vertically sliced spike of my weather app, Maeve, in order to get
a minimum viable product (MVP) up and running as quickly as possible. My focus is on designing
the user interface and simulating expected features so that I can dedicate the bulk of my time
implementing niche features that will attract users. For instance, I can use animations and
static data to mimic loading weather data from a remote API, but then actually implement
whatever needs to be implemented to have it tell the user what to wear that day.

#### Link to work

- [My GitHub Profile Page](https://github.com/webdavis/webdavis)
- [Maeve](https://github.com/webdavis/Maeve)

## Day 2: October 25, 2024

#### Today's Progress

1. Created a Bento profile.
1. Practiced implementing dependency diagrams.
1. Implemented a module-based dependency diagram for Maeve.

#### Thoughts on Architecture

Since Maeve’s current implementation is a vertically sliced spike, I’ll keep it as a monolith.
However, I can still maintain a logical separation of concerns through folder organization,
treating folders as “virtual modules”, so to speak. Since this is a spike, I can skip test
implementation, which allows me to avoid the delays caused by the simulator booting up with
UIKit.

#### Link to work

- [My Bento Profile](https://bento.me/webdavis)
- [Add module-based dependency diagram as SVG](https://github.com/webdavis/Maeve/commit/2290ab56eaf22adbc116bbb7bade15fd024a177b)
