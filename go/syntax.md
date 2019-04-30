## implement sqrt by newton method
```go
func Sqrt(x float64) float64 {
  // let initial guess to be 1
  z := 1.0
  for i := 1; i <= 10; i++ {
    z -= (z*z - x) / (2*z) // MAGIC LINE!!
    fmt.Println(z)
  }
  return z
}
```
