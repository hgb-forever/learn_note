# springboot错误笔记

①控制层要类前加上@Controller注解，以加入容器中

②@RequestMapping（value=“”）进行请求映射，如果需要返回内容再加入@Requestbody注解

③各组件应该放在springboot主程序的同包下，容器才能管理到

④自定义的国际化组件的BeanName必须是“localeResolver”

⑤页面发送跳转请求后，页面样式失效，是资源路径的问题，查找相关的样式路径，把相对路径改为绝对路径（加上"~/"）

⑥自己写的组件都应该放在主程序同包下