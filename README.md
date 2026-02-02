# Nuxt3 API Pokedex Assignment

## Project Description

This project involves creating a simple Pokedex app using the [PokeApi](https://pokeapi.co/). It's designed to help you become familiar with the Nuxt3 framework and practice essential frontend skills.

## Required Tools and Documentation

- [PokeApi](https://pokeapi.co/)
- [PokeApi Documentation](https://pokeapi.co/docs/v2)
- [Nuxt3 Documentation](https://v3.nuxtjs.org/)
- [Element Plus (for UI components)](https://element-plus.org/en-US/)

### Optional (Bonus Tools)

In our team, we frequently use the following tools. If you feel comfortable, try incorporating them into your project:

- [Pug (for templates)](https://pugjs.org/api/getting-started.html)
- [SCSS (for styling)](https://sass-lang.com/documentation)

You can install these tools with the following commands:

```bash
# Pug setup
pnpm i -D pug-plain-loader pug

# SCSS setup
pnpm i -D sass sass-loader
```

---

## Project Requirements

### Core Features

1. **Pokémon Search by Name**

   - Create an input field where users can search for a Pokémon by name.
   - Display the Pokémon's details (like name, type, height, weight, and description) in a card format, similar to the layout provided in the screenshot.

2. **Filter by Type**

   - Add buttons or checkboxes for each Pokémon type (e.g., Grass, Fire, Water, etc.).
   - Allow users to filter Pokémon by their type, displaying only those that match the selected type(s).
   - The filter should work alongside the search functionality, so users can search for a specific Pokémon within the selected type(s).

3. **Favorite Pokémon**
   - Implement a way for users to mark Pokémon as favorites. Each Pokémon card should have a heart icon that toggles when clicked.
   - Store favorite Pokémon locally using `localStorage` so that favorites persist across page reloads.
   - Add a "Favorites" section where users can view their saved Pokémon. Allow users to remove Pokémon from this list by clicking the heart icon again.

### UI and Layout

- Design the layout to resemble the sample provided, with a clean, user-friendly interface.
- Use Element Plus for consistent UI components (such as buttons, input fields, and cards).
- Try to match the layout and structure without copying the design exactly.

### Bonus (Optional)

- Use **Pug** for templates and **SCSS** for styling to practice these additional tools.
- Experiment with transitions or animations to make the user experience smoother, especially for adding/removing favorites.

---

## Submission Guidelines

1. **Repository**: Push your code to a Git repository and share the link.
2. **README**: Include a README.md file explaining the project setup, any configuration needed, and how to use the application.
3. **Screenshots**: Add screenshots of the main screens (search, filtered results, and favorites) to the README.

Good luck, and don't hesitate to reach out with any questions!
