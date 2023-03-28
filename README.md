# h
from flask import Flask
from flask_graphql import GraphQLView
from graphene import ObjectType, String, Int, List, Schema
from flask_cors import CORS
import json

app = Flask(__name__)
CORS(app)

class Todo(ObjectType):
    title = String()
    description = String()
    time = Int()

class Query(ObjectType):
    todos = List(Todo)

    def resolve_todos(self, info):
        # code to retrieve all todos
        return []

class Mutation(ObjectType):
    add_todo = Todo(title=String(), description=String(), time=Int())
    delete_todo = String(id=Int())
    edit_todo = Todo(id=Int(), title=String(), description=String(), time=Int())

    def resolve_add_todo(self, info, title, description, time):
        # code to add a todo
        return Todo(title=title, description=description, time=time)

    def resolve_delete_todo(self, info, id):
        # code to delete a todo
        return "Todo deleted"

    def resolve_edit_todo(self, info, id, title, description, time):
        # code to edit a todo
        return Todo(id=id, title=title, description=description, time=time)

schema = Schema(query=Query, mutation=Mutation)

app.add_url_rule(
    '/graphql',
    view_func=GraphQLView.as_view('graphql', schema=schema, graphiql=True)
)

if __name__ == '__main__':
    app.run(debug=True)

