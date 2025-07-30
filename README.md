<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Interactive Responsive Recipe Cards</title>
  <style>
    :root {
      --color-bg: #fefefe;
      --color-primary: #ff6f61;
      --color-secondary: #333;
      --color-text: #444;
      --color-card-bg: #fff;
      --color-card-shadow: rgba(0, 0, 0, 0.1);
      --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      --spacing: 1rem;
      --border-radius: 10px;
      --star-color: #ffbf00;
      --star-gray: #ccc;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: var(--font-family);
      background: var(--color-bg);
      color: var(--color-text);
      line-height: 1.5;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    header {
      padding: var(--spacing);
      text-align: center;
      background-color: var(--color-primary);
      color: white;
      font-size: 1.8rem;
      font-weight: 700;
      letter-spacing: 1px;
      user-select: none;
    }

    .filter-bar {
      max-width: 1200px;
      margin: 1rem auto 0 auto;
      padding: 0 var(--spacing);
      display: flex;
      justify-content: flex-end;
      gap: 0.5rem;
      align-items: center;
    }

    .filter-bar label {
      font-weight: 600;
      font-size: 1rem;
      color: var(--color-secondary);
      user-select: none;
    }

    .filter-bar select {
      padding: 0.3rem 0.5rem;
      border-radius: 5px;
      border: 1.5px solid var(--color-primary);
      font-size: 1rem;
      cursor: pointer;
      transition: border-color 0.3s ease;
    }

    .filter-bar select:hover,
    .filter-bar select:focus {
      border-color: #ff8a75;
      outline: none;
    }

    main {
      max-width: 1200px;
      margin: 1rem auto 3rem auto;
      padding: 0 var(--spacing);
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 2rem;
      flex-grow: 1;
    }

    article.recipe-card {
      background: var(--color-card-bg);
      border-radius: var(--border-radius);
      box-shadow: 0 2px 15px var(--color-card-shadow);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      transition: background-color 0.3s ease, transform 0.3s ease;
      cursor: default;
      position: relative;
    }

    article.recipe-card:hover {
      background-color: #fff4f2;
      transform: translateY(-4px);
      box-shadow: 0 6px 20px rgba(255, 111, 97, 0.35);
      z-index: 2;
    }

    .recipe-image {
      width: 100%;
      aspect-ratio: 4 / 3;
      object-fit: cover;
      transition: transform 0.3s ease;
      border-top-left-radius: var(--border-radius);
      border-top-right-radius: var(--border-radius);
    }

    article.recipe-card:hover .recipe-image {
      transform: scale(1.05);
    }

    .recipe-content {
      padding: var(--spacing);
      flex-grow: 1;
      display: flex;
      flex-direction: column;
    }

    h2.recipe-title {
      margin: 0 0 0.25rem 0;
      color: var(--color-primary);
      font-size: 1.5rem;
    }

    p.recipe-description {
      font-style: italic;
      margin: 0 0 1rem 0;
      color: #666;
      flex-grow: 0;
    }

    section.ingredients,
    section.preparation {
      margin-bottom: 1rem;
      user-select: text;
    }

    section.ingredients ul {
      padding-left: 1.25rem;
      margin: 0;
      list-style-type: disc;
      max-height: 6rem;
      overflow: hidden;
      transition: max-height 0.3s ease;
    }

    section.ingredients.expanded ul {
      max-height: 1000px;
    }

    button.toggle-btn {
      background: var(--color-primary);
      color: white;
      border: none;
      padding: 0.3rem 0.6rem;
      border-radius: 5px;
      cursor: pointer;
      font-size: 0.85rem;
      align-self: flex-start;
      transition: background-color 0.2s ease;
      user-select: none;
      margin-top: 0.4rem;
    }

    button.toggle-btn:hover {
      background-color: #e6554f;
    }

    section.preparation p {
      margin: 0;
      user-select: text;
    }

    /* Rating stars */
    .rating {
      margin-top: auto;
      user-select: none;
    }

    .stars {
      display: inline-flex;
      gap: 0.15rem;
      cursor: pointer;
    }

    .stars svg {
      width: 24px;
      height: 24px;
      fill: var(--star-gray);
      transition: fill 0.3s ease;
    }

    .stars svg.filled {
      fill: var(--star-color);
    }

    .stars svg:hover,
    .stars svg:hover ~ svg {
      fill: var(--star-gray);
    }

    .stars svg:hover,
    .stars svg:hover ~ svg {
      fill: var(--star-gray);
    }

    /* Responsive tweaks */
    @media (max-width: 900px) {
      main {
        grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
        gap: 1.5rem;
      }
    }

    @media (max-width: 600px) {
      main {
        grid-template-columns: 1fr;
        margin: 1rem;
      }
      .filter-bar {
        justify-content: center;
        margin-bottom: 0.5rem;
      }
    }
  </style>
</head>
<body>

<header>Delicious Recipes</header>

<div class="filter-bar" aria-label="Filter recipes by cuisine">
  <label for="cuisineFilter">Filter by Cuisine:</label>
  <select id="cuisineFilter" aria-controls="recipeList" aria-label="Select cuisine filter">
    <option value="all">All</option>
    <option value="italian">Italian</option>
    <option value="mexican">Mexican</option>
    <option value="american">American</option>
    <option value="indian">Indian</option>
  </select>
</div>

<main id="recipeList">

  <article class="recipe-card" data-cuisine="italian" aria-label="Recipe for Spaghetti Carbonara">
    <img
      src="https://images.unsplash.com/photo-1604908177524-923e0b0d5b5b?auto=format&fit=crop&w=800&q=80"
      alt="Spaghetti Carbonara" class="recipe-image" />
    <div class="recipe-content">
      <h2 class="recipe-title">Spaghetti Carbonara</h2>
      <p class="recipe-description">
        Classic Italian pasta with creamy egg and pancetta sauce.
      </p>
      <section class="ingredients" aria-label="Ingredients for Spaghetti Carbonara">
        <strong>Ingredients:</strong>
        <ul>
          <li>200g spaghetti</li>
          <li>100g pancetta</li>
          <li>2 large eggs</li>
          <li>50g Pecorino cheese</li>
          <li>Black pepper</li>
          <li>Salt</li>
          <li>1 clove garlic (optional)</li>
        </ul>
        <button class="toggle-btn" aria-expanded="false">Show More</button>
      </section>
      <section class="preparation" aria-label="Preparation instructions for Spaghetti Carbonara">
        <strong>Preparation:</strong>
        <p>
          Cook spaghetti until al dente. Fry pancetta until crisp. Beat eggs with cheese and pepper.
          Combine pasta with pancetta and remove from heat, quickly stir in egg mixture to create a creamy sauce.
          Serve immediately.
        </p>
      </section>
      <div class="rating" aria-label="Recipe rating">
        <strong>Rate this recipe:</strong>
        <div class="stars" role="radiogroup" aria-labelledby="rating-label-1">
          <svg role="radio" tabindex="0" aria-checked="false" aria-label="1 star" data-star="1" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="2 stars" data-star="2" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="3 stars" data-star="3" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="4 stars" data-star="4" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="5 stars" data-star="5" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
        </div>
      </div>
    </div>
  </article>

  <article class="recipe-card" data-cuisine="american" aria-label="Recipe for Avocado Toast">
    <img
      src="https://images.unsplash.com/photo-1551183053-bf91a1d81141?auto=format&fit=crop&w=800&q=80"
      alt="Avocado Toast" class="recipe-image" />
    <div class="recipe-content">
      <h2 class="recipe-title">Avocado Toast</h2>
      <p class="recipe-description">
        Quick and healthy avocado spread on toasted bread.
      </p>
      <section class="ingredients" aria-label="Ingredients for Avocado Toast">
        <strong>Ingredients:</strong>
        <ul>
          <li>2 slices of whole grain bread</li>
          <li>1 ripe avocado</li>
          <li>Salt and pepper</li>
          <li>Red pepper flakes (optional)</li>
          <li>Lemon juice</li>
          <li>Olive oil</li>
          <li>Cherry tomatoes (optional)</li>
        </ul>
        <button class="toggle-btn" aria-expanded="false">Show More</button>
      </section>
      <section class="preparation" aria-label="Preparation instructions for Avocado Toast">
        <strong>Preparation:</strong>
        <p>
          Toast the bread slices. Mash avocado with lemon juice, salt, and pepper. Spread over toast.
          Drizzle olive oil and sprinkle red pepper flakes and cherry tomatoes if desired.
        </p>
      </section>
      <div class="rating" aria-label="Recipe rating">
        <strong>Rate this recipe:</strong>
        <div class="stars" role="radiogroup" aria-labelledby="rating-label-2">
          <svg role="radio" tabindex="0" aria-checked="false" aria-label="1 star" data-star="1" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="2 stars" data-star="2" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="3 stars" data-star="3" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="4 stars" data-star="4" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
          <svg role="radio" tabindex="-1" aria-checked="false" aria-label="5 stars" data-star="5" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2 10.19,8.62 3,9.24 8.46,13.97 5.82,21" />
          </svg>
        </div>
      </div>
    </div>
  </article>

  <article class="recipe-card" data-cuisine="indian" aria-label="Recipe for Chicken Curry">
    <img
      src="https://images.unsplash.com/photo-1605479390375-0ed086dd79f3?auto=format&fit=crop&w=800&q=80"
      alt="Chicken Curry" class="recipe-image" />
    <div class="recipe-content">
      <h2 class="recipe-title">Chicken Curry</h2>
      <p class="recipe-description">
        Spicy and flavorful chicken curry with rich sauce.
      </p>
      <section class="ingredients" aria-label="Ingredients for Chicken Curry">
        <strong>Ingredients:</strong>
        <ul>
          <li>500g chicken pieces</li>
          <li>2 onions, sliced</li>
          <li>3 garlic cloves</li>
          <li>1 tbsp ginger paste</li>
          <li>2 tbsp curry powder</li>
          <li>400ml coconut milk</li>
          <li>Salt to taste</li>
          <li>Fresh coriander</li>
        </ul>
        <button class="toggle-btn" aria-expanded="false">Show More</button>
      </section>
      <section class="preparation" aria-label="Preparation instructions for Chicken Curry">
        <strong>Preparation:</strong>
        <p>
          Sauté onions, garlic, and ginger. Add chicken and brown. Stir in curry powder and coconut milk.
          Simmer until chicken is cooked and sauce thickens. Garnish with coriander and serve with rice.
        </p>
      </section>
      <div class="rating" aria-label="Recipe rating">
        <strong>Rate this recipe:</strong>
        <div class="stars" role="radiogroup" aria-labelledby="rating-label-3">
          <svg role="radio" tabindex="0" aria-checked="false" aria-label="1 star" data-star="1" viewBox="0 0 24 24" >
            <polygon points="12,17.27 18.18,21 15.54,13.97 21,9.24 13.81,8.62 12,2
