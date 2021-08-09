# To-do App

1. Clone down the repository

    ```bash
    git clone https://github.com/angelh84/to-do-app.git
    ```

2. Enter the repo directory

    ```bash
    cd to-do-app
    ```

3. Install the dependencies (requires Node v. >=11.10.1)

    ```bash
    npm install
    ```

4. Run the app

    ```bash
    npm run serve
    ```


## Features

1. Users can add as many to-do items as they wish.

2. Users can check off their item.

3. Any checked items can be edited inline when clicking on the 'Edit Selected' button that appears when at least one item is checked. (The edit button toggles the checked item state).

4. Any checked items can be deleted via the 'Delete Selected' button that appears when at least one item is checked.

5. All items can be selected or deselected by clicking on the checkbox next to the app headline.

6. Lists can be sorted by selecting a priority under the 'Sort' drop down to the right of the app headline.

7. Data is saved to local storage any time the main data object is modified.


## Overview

a. For quick prototyping, I used Tailwind as a dependency, which allows for quicker manipulation of form styling as well as cleaner and more consistent CSS. (no custom css was written).

b. I started conceptualizing this to-do app by wireframing some ideas and went back and forth between creating a list by priority or creating one master list with items tagged with priority -- I ended up doing the latter.

c. I began the markup and styling for mobile and gradually moved up to desktop. As I was doing this, I moved a few elements around on mobile, including the header buttons that appear when selecting an item as well changing the priority select drop down into radio buttons. 

b. I worked on the logic for selcting / deselecting all, deleting and editing items.  

c. Added sorting by priority.

d. Added the ability to save data to local storage.


## Conclusion

Thanks for reading.
