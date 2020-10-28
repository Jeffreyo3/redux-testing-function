# redux-testing-function

Created to be used with react-test-renderer
Should be able to be used with any React component
 
# How to use

Import `{ findElementById }`.\
When needing to find an element by id, pass in the root component & desired id.\
Example:
```
describe('UserEditActive with item.value as an array', () => {

    let store;
    let component;
    const item = {
        icon: "ban"
    };

    const options = [
        { text: "Vegan", value: "vegan" },
        { text: "Gluten-Free", value: "gluten-free" },
        { text: "Lacto-vegetarian", value: "lacto-vegetarian" },
        { text: "Ovo-vegetarian", value: "ovo-vegetarian" },
        { text: "Pescetarian", value: "pescetarian" },
        { text: "Vegetarian", value: "vegetarian" },
        { text: "None", value: "" },
    ]

    beforeEach(() => {
        store = mockStore({
            user: { field: 'dietary_preference' }
        });

        store.dispatch = jest.fn();

        component = renderer.create(
            <Provider store={store}>
                <UserEditActive item={item} />
            </Provider>
        )
    });

    it('should render with text from props', () => {
        const dropdown = findElementById(component, 'dropdown')  /* <-------- EXAMPLE IMPLEMENTATION */
        expect(dropdown.name).toBe('dietary_preference')

        // find the dropdown items
        const menuItems = dropdown.children.filter(({ props }) => props.className && props.className === 'menu transition')[0]
        // loop through items & make sure they match incoming prop text
        menuItems.children.forEach((item, index) => {
            expect(item.children[0].children[0]).toContain(options[index].text)
        })
    })

```
