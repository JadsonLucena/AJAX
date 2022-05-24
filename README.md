# Ajax.js
Through this facade, get all XHR events on promises or callbacks

## Which is?
The XMLHttpRequest object can be used to exchange data with a server behind the scenes. This means that it is possible to update parts of a web page without having to reload the entire page, therefore, without interrupting what the user is doing.


## Interface
```javascript
Ajax(
    url: string,
    {
        async = true,
        body = null,
        headers = {},
        method = 'GET',
        mimeType = 'text/plain', // type/subtype;parameter=value
        password = null,
        responseType = 'text',
        timeout = 0, // Time in milliseconds
        user = null,
        withCredentials = false,
        aborted = () => {},
        end = () => {},
        error = () => {},
        progress = () => {},
        readystate = () => {},
        start = () => {},
        success = () => {},
        timeouted = () => {},
        XHR = () => {}
    }: {
        async?: boolean,
        body?: ArrayBuffer | ArrayBufferView | Blob | BufferSource | Document | FormData | ReadableStream<any> | string | URLSearchParams | null,
        headers?: Headers | { [ string: string ]: string },
        method?: 'CONNECT' | 'DELETE' | 'GET' | 'HEAD' | 'OPTIONS' | 'PATCH' | 'POST' | 'PUT' | 'TRACE',
        mimeType?: string,
        password?: string | null,
        responseType?: 'arraybuffer' | 'blob' | 'document' | 'json' | 'text',
        timeout?: number,
        user?: string | null,
        withCredentials?: boolean,
        aborted?: (timeStamp: number) => void,
        end?: (timeStamp: number) => void,
        error?: (error: {readyState: 1 | 2 | 3 | 4, status: number, statusText: string, timeStamp: number, type: string, XHR: XMLHttpRequest}) => void,
        progress?: (progress: {lengthComputable: boolean, loaded: number, total: number, timeStamp: number}) => void,
        readystate?: (readystate: {readyState: 1 | 2 | 3 | 4, status: number, statusText: string, timeStamp: number}) => void,
        start?: (timeStamp: number) => void,
        success?: (success: {readyState: 1 | 2 | 3 | 4, status: number, statusText: string, timeStamp: number, getAllResponseHeaders: XMLHttpRequest['getAllResponseHeaders'], getResponseHeader: XMLHttpRequest['getResponseHeader'], response: XMLHttpRequest['response'], XHR: XMLHttpRequest}) => void,
        timeouted?: (timeStamp: number) => void,
        XHR?: (XHR: {abort: XMLHttpRequest['abort'], XHR: XMLHttpRequest}) => void
    } = {}
): Promise<{readyState: 1 | 2 | 3 | 4, status: number, statusText: string, timeStamp: number, getAllResponseHeaders: XMLHttpRequest['getAllResponseHeaders'], getResponseHeader: XMLHttpRequest['getResponseHeader'], response: XMLHttpRequest['response'], XHR: XMLHttpRequest}>
```

## How to use
```html
<script src="https://cdn.jsdelivr.net/gh/JadsonLucena/Ajax@latest/src/Ajax.js"></script>
<script>
// Simple GET
    // Callback
        Ajax('/path', {
            success: e => console.log('Success', e.readyState, e.status, e.statusText, e.timestamp, e.getAllResponseHeaders(), e.getResponseHeader('content-type'), e.response, e.XHR),
            error: e => console.log('Error', e.readyState, e.status, e.statusText, e.timestamp, e.type, e.XHR)
        });

    // Promise
        Ajax('/path')
            .then(e => console.log('Success', e.readyState, e.status, e.statusText, e.timestamp, e.getAllResponseHeaders(), e.getResponseHeader('content-type'), e.response, e.XHR))
            .catch(e => console.log('Error', e.readyState, e.status, e.statusText, e.timestamp, e.type, e.XHR));

// Simple POST
Ajax('/path', {
    method: 'POST',
    body: 'Hello World'
})
    .then(e => console.log('Success', e))
    .catch(e => console.log('Error', e));

// Send and Receive JSON
Ajax('/path', {
    method: 'POST',
    headers: {
        'Content-type': 'application/json',
        'Accept': 'application/json'
    },
    responseType: 'json',
    body: JSON.stringify({ 'content': 'Hello World' })
})
    .then(e => console.log('Success', e))
    .catch(e => console.log('Error', e));

// Upload Progress
Ajax('/formHandler', {
    method: 'POST',
    headers: {
        'Content-Type': 'multipart/form-data'
    },
    body: new FormData(),
    progress: e => console.log('progress', e, parseInt(e.loaded / e.total * 100) +'%')
}).then(e => console.log('Success', e)).catch(e => console.log('Error', e));

// Download Progress
Ajax('/path', {
    responseType: 'arraybuffer',
    progress: e => console.log('progress', e, parseInt(e.loaded / e.total * 100) +'%')
}).then(e => console.log('Success', e)).catch(e => console.log('Error', e));

// Abort request
Ajax('/path', {
    XHR: XHR => setTimeout(() => XHR.abort(), 100)
}).then(e => console.log('Success', e)).catch(e => console.log('Error', e));

// Credentials
let login = 'any';
let password = 'any';
Ajax('/path', {
    withCredentials: true,
    headers: {
        'Authorization': 'Basic '+ btoa(login +':'+ password)
    }
}).then(e => console.log('Success', e)).catch(e => console.log('Error', e));
</script>
```

### References

> [XMLHttpRequest WHATWG](https://xhr.spec.whatwg.org)\
> [XMLHttpRequest MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

> [Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers)\
> [MimeType](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)\
> [ResponseType](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseType)\
> [Timeout](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/timeout)\
> [WithCredentials](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/withCredentials)
