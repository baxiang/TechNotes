```
val result = try {
  Integer.parseInt("bar")
} catch {
  case _: Throwable => 1
}finally {
  println("finally println")
}


```
