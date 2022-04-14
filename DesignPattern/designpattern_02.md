# Design Pattern_02

## MVC Pattern

![Diagram to show the different parts of the mvc architecture.](https://developer.mozilla.org/en-US/docs/Glossary/MVC/model-view-controller-light-blue.png)

-   **MVC** (Model-View-Controller) is a pattern in software design commonly used to implement user interfaces, data, and controlling logic.
-   **It emphasizes a separation between the software's business logic and display.**

### Model: Manages data and business logic.

-   **The model defines what data the app should contain.**
-   If the state of this data changes, then the model will usually notify the view (so the display can change as needed) and sometimes the controller (if different logic is needed to control the updated view).

### View: Handles layout and display.

-   **The view defines how the app's data should be displayed.**

### Controller: Routes commands to the model and view parts.

-   **The controller contains logic that updates the model and/or view in response to input from the users of the app.**
-   So for example, our shopping list could have input forms and buttons that allow us to add or delete items. These actions require the model to be updated, so the input is sent to the controller, which then manipulates the model as appropriate, which then sends updated data to the view.
-   You might however also want to just update the view to display the data in a different format, e.g., change the item order to alphabetical, or lowest to highest price. In this case the controller could handle this directly without needing to update the model.

### When?

-   This "separation of concerns" provides for a **better division of labor and improved maintenance.**

### Advantages

-   **Controller만으로** 그 app의 로직을 이해할 수 있다.
-   코드가 간결해진다 (유지보수 용이성)
-   규모가 작은 앱일수록 효과적
-   iOS - MVC, Android - MVVM

### Disadvantages

-   컨트롤러가 모든걸 다 들고 있는 형태가 되어서 컨트롤러가 순식간에 커질 수 있음.
-   흔히 Massive View Controller라고 표현하는 상태.



## Delegation Pattern

![Delegation Pattern](https://t1.daumcdn.net/thumb/R720x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/aUYX/image/mO9Z5HmA2jwcki0Th9-XNZ73Xz0.png)

-   Object-oriented design pattern that allows object composition to achieve the same code reuse as inheritance.
-   **Enables object to use another "helper" object to provide data or perform a task.**

### The delegate

-   An object handles a request by delegating to a second object
-   helper object, *but with the original context.*

### Receiving object

-   Have `self` in the delegate refer to the original (sending) object, not the delegate (receiving object).

### When?

-   Note that "delegation" is **often used loosely to refer to the distinct concept of forwarding,** where the sending object simply uses the corresponding member on the receiving object, evaluated in the context of the *receiving* object, not the original object.

### Advantages

-   MVC Pattern에서, View가 이벤트를 받은 경우, View에서 이에 대한 로직을 구현하면 OOP의 단일 책임 원칙을 위반하게 된다.
    -   따라서 interface (or protocol)를 통해 controller로 이를 넘겨주고, controller에서 이에 대한 로직을 구현해 문제를 해결하는 것.
-   MVC에서 필연적으로 사용되는 패턴

