### 1.4 put async actions into componentDidMount

Sometimes your component has to fetch data. So where do you put asynchronous actions in your react component? The answer is: componentDidMount

There are actually "three" possible places:
1. componentWillMount
2. constructor
3. componentDidMount

the ES6 style constructor method is actually the same as componentWillMount so you can put your code there instead of
creating the componentWillMount lifecycle hook.

But the problem with componentWillMount is that the component actually will render at least one time before your data is fetched. This can create some misunderstanding because some developers might think they don't need to take care of the components default state because the data "should" already be there before the initial render.

But there are two reasons to put async actions into componentDidMount:
1. it makes clear that the component already mounted so you need to take care of the state of your component before the data is fetched
2. if you want to do SSR componentWillMount can be fired multiple times so it's a good idea to get used to put your code into cDM 

Additional reading:
* [David Ceddia - daveceddia.com](https://daveceddia.com/where-fetch-data-componentwillmount-vs-componentdidmount/?utm_content=buffer8ffb3&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)
