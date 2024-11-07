
**computed是响应式的,** 给computed设置get和set会和Object.defineProperty关联起来(vue2),vue3会和proxy关联

**computed是有缓存的**，主要通过dirty控制

dirty是一个脏数据标志位，为false时表示读取computed使用缓存，为true时表示读取computed会执行get函数重新计算

