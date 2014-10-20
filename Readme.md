# TreeRoute #

This message handler is an an alternative to using the standard HttpRoute class in a Web API project to route requests to controllers.


Basic usage:

You can build the tree manually in a similar way you build XDocuments, e.g.

       var treeRoute = new TreeRoute("api",
                                     new TreeRoute("Contact",
                                                   new TreeRoute("{id}").To<FakeController>()));

to use the route, simply add it to your route collection.

       config.Routes.Add("mytreeroute",treeRoute);


You can also build the trees in a way that is similar to regular routes.  e.g.

            var route = new TreeRoute("games");
            route.AddWithPath("{gametitle}/Setup/{gamesid}", r => r.To<SetupController>());
            route.AddWithPath("{gametitle}/Resources/{resourcetype}/{resourceid}", r => r.To<ResourceController>());
            route.AddWithPath("{gametitle}/{gameid}/Chat/{chatid}", r => r.To<ChatController>());
            route.AddWithPath("{gametitle}/{gameid}/State/{stateid}", r => r.To<StateController>());

Behind the scenes we do actually build a tree of TreeRoutes based on the paths provided.

One advantage of building a tree of routes is that for large and deep trees, the performance of route resolution should be significantly better.  Also, future versions will allow attaching message handlers to the tree and routing resolution will generate a custom pipeline of message handlers that are specific to that route.

Currently, RouteConstraints and RouteDefaults are not implemented.

Here is and example from some simple demo APIs I have built,

            var route = new TreeRoute("");

            route.AddWithPath("switch", r => r.To<SwitchController>());
            route.AddWithPath("switch/on", r => r.To<SwitchController>().ToAction("on"));
            route.AddWithPath("switch/off", r => r.To<SwitchController>().ToAction("off"));

            route.AddWithPath("expenseapp", r => r.To<ExpenseAppController>());
            route.AddWithPath("expenses", r => r.To<ExpensesController>());
            route.AddWithPath("expense/{expenseId}", r => r.To<ExpenseController>());
            route.AddWithPath("expense/{expenseId}/approve", r => r.To<ExpenseController>().ToAction("Approve"));
            route.AddWithPath("expense/{expenseId}/reject", r => r.To<ExpenseController>().ToAction("Reject"));
            route.AddWithPath("receipt", r => r.To<ReceiptController>());


            config.Routes.Add("",route);
