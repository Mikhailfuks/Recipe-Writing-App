using System;
using System.Collections.Generic;

namespace RecipeApp
{
    // Класс для представления рецепта
    public class Recipe
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public List<Ingredient> Ingredients { get; set; }
        public List<Instruction> Instructions { get; set; }
        public int CookingTime { get; set; } // Время приготовления в минутах
        public int Servings { get; set; }

        // Конструктор
        public Recipe(int id, string name, string description, List<Ingredient> ingredients, 
                      List<Instruction> instructions, int cookingTime, int servings)
        {
            Id = id;
            Name = name;
            Description = description;
            Ingredients = ingredients;
            Instructions = instructions;
            CookingTime = cookingTime;
            Servings = servings;
        }
    }

    // Класс для представления ингредиента
    public class Ingredient
    {
        public string Name { get; set; }
        public decimal Quantity { get; set; }
        public string Unit { get; set; } // Единица измерения (например, "г", "мл", "шт.")

        // Конструктор
        public Ingredient(string name, decimal quantity, string unit)
        {
            Name = name;
            Quantity = quantity;
            Unit = unit;
        }
    }

    // Класс для представления инструкции
    public class Instruction
    {
        public string Text { get; set; }

        // Конструктор
        public Instruction(string text)
        {
            Text = text;
        }
    }

    // Класс для управления рецептами
    public class RecipeManager
    {
        private List<Recipe> recipes = new List<Recipe>();
        private int nextRecipeId = 1;

        // Добавление рецепта
        public void AddRecipe(string name, string description, List<Ingredient> ingredients,
                              List<Instruction> instructions, int cookingTime, int servings)
        {
            recipes.Add(new Recipe(nextRecipeId++, name, description, ingredients, 
                                   instructions, cookingTime, servings));
        }

        // Получение списка рецептов
        public List<Recipe> GetRecipes()
        {
            return recipes;
        }

        // Поиск рецепта по имени
        public Recipe FindRecipeByName(string name)
        {
            return recipes.Find(r => r.Name == name);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Создание менеджера рецептов
            RecipeManager recipeManager = new RecipeManager();

            // Добавление рецептов
            // Рецепт 1: Пицца
            List<Ingredient> pizzaIngredients = new List<Ingredient>()
            {
                new Ingredient("Мука", 250, "г"),
                new Ingredient("Дрожжи", 7, "г"),
                new Ingredient("Вода", 150, "мл"),
                new Ingredient("Соль", 5, "г"),
                new Ingredient("Томатный соус", 100, "мл"),
                new Ingredient("Моцарелла", 150, "г"),
                new Ingredient("Пепперони", 100, "г"),
                new Ingredient("Лук", 1, "шт."),
                new Ingredient("Перец", 1, "шт.")
            };
            List<Instruction> pizzaInstructions = new List<Instruction>()
            {
                new Instruction("Смешать муку, дрожжи, воду и соль. Замесить тесто."),
                new Instruction("Раскатать тесто и выложить на противень."),
                new Instruction("Смазать тесто томатным соусом."),
                new Instruction("Посыпать моцареллой, пепперони, луком и перцем."),
                new Instruction("Выпекать в духовке при 200°C 15-20 минут.")
            };
            recipeManager.AddRecipe("Пицца", "Классическая пицца с пепперони", pizzaIngredients, pizzaInstructions, 20, 4);

            // Рецепт 2: Салат
            List<Ingredient> saladIngredients = new List<Ingredient>()
            {
                new Ingredient("Салатные листья", 100, "г"),
                new Ingredient("Помидоры", 2, "шт."),
                new Ingredient("Огурцы", 1, "шт."),
                new Ingredient("Кукуруза", 50, "г"),
                new Ingredient("Майонез", 2, "ст.л.")
            };
            List<Instruction> saladInstructions = new List<Instruction>()
            {
                new Instruction("Нарезать помидоры и огурцы."),
                new Instruction("Смешать салатные листья, помидоры, огурцы и кукурузу."),
                new Instruction("Заправить майонезом.")
            };
            recipeManager.AddRecipe("Салат", "Простой и вкусный салат", saladIngredients, saladInstructions, 10, 2);

            // Вывод списка рецептов
            Console.WriteLine("Список рецептов:");
            foreach (Recipe recipe in recipeManager.GetRecipes())
            {
                Console.WriteLine($"{recipe.Id}. {recipe.Name} ({recipe.CookingTime} мин, {recipe.Servings} порции)");
            }

            // Поиск рецепта по имени
            Console.Write("\nВведите название рецепта для просмотра: ");
            string recipeName = Console.ReadLine();
            Recipe foundRecipe = recipeManager.FindRecipeByName(recipeName);

            if (foundRecipe != null)
            {
                Console.WriteLine($"\nНазвание: {foundRecipe.Name}");
                Console.WriteLine($"Описание: {foundRecipe.Description}");
                Console.WriteLine("\nИнгредиенты:");
                foreach (Ingredient ingredient in foundRecipe.Ingredients)
                {
                    Console.WriteLine($"{ingredient.Quantity} {ingredient.Unit} {ingredient.Name}");
                }
                Console.WriteLine("\nИнструкции:");
                foreach (Instruction instruction in foundRecipe.Instructions)
                {
                    Console.WriteLine($"{instruction.Text}");
                }
            }
            else
            {
                Console.WriteLine("Рецепт не найден.");
            }

            Console.ReadKey();
        }
    }
}
