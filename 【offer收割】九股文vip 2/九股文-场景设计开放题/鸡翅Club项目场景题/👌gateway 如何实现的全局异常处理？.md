gateway 的全局异常处理主要需要我们实现一个接口ErrorWebExceptionHandler，实现其中的 handle 方法，在方法内，我们能获取到其中 request 与 response，webhandler 会帮助我们拦住所有异常的情况。然后我们可以在里面做拦截的进一步处理，更改状态码，状态错误信息等等。最后通过 response 可以将其返回出去。
