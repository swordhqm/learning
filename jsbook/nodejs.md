# NodeJS

---

获取当前命令执行路径

```
process.cwd()
```

---

解析软链接或者目录 为真实目录

```
const appDirectory = fs.realpathSync(process.cwd());
```

由于linux 和windows 的path在表达上存在不一致，所以通过process.cwd\(\)来获取一个运行时的目录，然后通过path操作来满足多平台的适应性

relativePath 比如 ./tmp，../../tmp

