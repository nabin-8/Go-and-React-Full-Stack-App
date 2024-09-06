# Go-and-React-Full-Stack-App
Go and React Full Stack App

### Initialized Go Project
```go
go mod init <module_name>

go mod init github.com/nabin-8/Go-and-React-Full-Stack-App
```
- It will generate go.mod file
- A package is a collection of Go source files that reside in the same directory and have the same package declaration at the top.
- Packages collectively form a module

#### Install Fiber Framework
```go
 go get github.com/gofiber/fiber/v2
```
#### Install air in go
```go
go install github.com/air-verse/air@latest
```
#### Install .env in go
```go
go get github.com/joho/godotenv
```
#### CRUD without Database
```go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/gofiber/fiber/v2"
	"github.com/joho/godotenv"
)

type Todo struct {
	ID        int    `json:"id"`
	Completed bool   `json:"completed"`
	Body      string `json:"body"`
}

func main() {
	app := fiber.New()
	err := godotenv.Load(".env")
	if err != nil {
		log.Fatal("Error loading .env file")
	}
	PORT := os.Getenv("PORT")
	todos := []Todo{}

	// View Todos
	app.Get("/api/todos", func(c *fiber.Ctx) error {
		return c.Status(200).JSON(todos)
	})

	// Create a Todo
	app.Post("/api/todos", func(c *fiber.Ctx) error {
		todo := &Todo{} // {id:0, completed:false}
		if err := c.BodyParser(todo); err != nil {
			return err
		}

		if todo.Body == "" {
			return c.Status(400).JSON(fiber.Map{"error": "Todo body is required"})
		}

		todo.ID = len(todos) + 1
		todos = append(todos, *todo)

		return c.Status(201).JSON(todo)

	})

	// Update a todo
	app.Patch("/api/todos/:id", func(c *fiber.Ctx) error {
		id := c.Params("id")

		for i, todo := range todos {
			if fmt.Sprint(todo.ID) == id {
				todos[i].Completed = true
				return c.Status(200).JSON(todos[i])
			}

		}
		return c.Status(404).JSON(fiber.Map{"error": "Todo not found"})
	})

	// Delete a Todo
	app.Delete("/api/todos/:id", func(c *fiber.Ctx) error {
		id := c.Params("id")
		c.Params("id")
		for i, todo := range todos {
			if fmt.Sprint(todo.ID) == id {
				todos = append(todos[:i], todos[i+1:]...)
				return c.Status(200).JSON(fiber.Map{"success": true})
			}
		}
		return c.Status(404).JSON(fiber.Map{"error": "Todo not found"})
	})

	fmt.Println("Server is running on port: " + PORT)
	log.Fatal(app.Listen(":" + PORT))
}
```
#### Let's Work with MongoDB
Install mongodb driver
```go
go get go.mongodb.org/mongo-driver/mongo
```
#### New let's work on frontend
```
mkdir frontend 
cd frontend
npm create vite@latest .
```
- React
- Typescript and
npm i
