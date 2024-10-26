# 100 Days Of Code - Log

## Day 1: October 24, 2024

### Today's Progress

1. Created my GitHub Profile page.
1. Revisited building prototypes using Xcode's Interface Builder and faking implementation
   details using animations.

### Thoughts on GitHub Profile pages

I enjoyed creating my GitHub Profile page. Writing copy takes way more brain power than I
expected it would—just thinking about word choice and the different tones they evoke.

### Thoughts on Prototyping

I’m currently implementing a vertically sliced spike of my weather app, Maeve, in order to get
a minimum viable product (MVP) up and running as quickly as possible. My focus is on designing
the user interface and simulating expected features so that I can dedicate the bulk of my time
implementing niche features that will attract users. For instance, I can use animations and
static data to mimic loading weather data from a remote API, but then actually implement
whatever needs to be implemented to have it tell the user what to wear that day.

### Link to work

- [My GitHub Profile Page](https://github.com/webdavis/webdavis)
- [Maeve](https://github.com/webdavis/Maeve)

## Day 2: October 25, 2024

### Today's Progress

1. Created a Bento profile.
2. Practiced implementing dependency diagrams.
3. Implemented a module-based dependency diagram for Maeve.

### Thoughts on Architecture

For this round I'm implementing a vertically sliced spike so that I know what is and isn't
possible for this app.

As a result, I won't be test-driving it. That's actually beneficial here because for UI modules
Xcode boots the simulator every time tests are run, which makes development painfully slow.
(Not writing tests avoids that 🤣.)

↕️  Since it's a vertical slice, a monolithic architecture makes the most sense.

💡 However, that doesn't mean I won't attempt to make thoughtful architectural decisions.

Even though the code won't be divided into separate physical modules, I can still organize
concerns using folders and treat them as "virtual modules." For example, `<Weather Service>` is
a clear boundary that facilitates communication between the other modules.

### Link to work

- [My Bento Profile](https://bento.me/webdavis)
- [Add module-based dependency diagram as SVG](https://github.com/webdavis/Maeve/commit/2290ab56eaf22adbc116bbb7bade15fd024a177b)
