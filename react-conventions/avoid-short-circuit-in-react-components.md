Avoid the short circuit pattern to show / hide components based on boolean expressions. 

- Performance: Add or Remove components in a long component tree creates a performance problem
- Ideally, the responsibility for rendering should be as deep as possible on the component tree. 
- Initial loading time can be use to render components before the application becomes usable

Ref: https://17.reactjs.org/docs/concurrent-mode-suspense.html

### Bad Code

```js
const Modal = () =>{};

const View = () => {
    const [showModal,setShowModal] = useState();

    return (
              showModal && <Modal />
    );
}

const View = () => {
    const [loading,setLoading] = useState(true);
    useEffect(()=>{
        ...
        setLoading(false);
    });

   if(loading === true)
     return <Loading />

    return (
              <>
                    ....
              <>
    );
}


```

### Good Code

```js
const Modal = ({show}) =>{};

const View = () => {
    const [showModal,setShowModal] = useState();

    return (
             <Modal show={showModal} />
    );
}

const View = () => {
    const [loading,setLoading] = useState(true);
    useEffect(()=>{
        ...
        setLoading(false);
    });
    return (
               <Loading loading={loading} />
              <>
                    <Table loading={loading}>
                    </Table>
              <>
    );
}

```
