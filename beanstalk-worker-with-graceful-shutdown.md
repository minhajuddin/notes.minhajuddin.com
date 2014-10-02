# Beanstalk worker with graceful shutdown


In golang

    package main

    import (
      "fmt"
      "github.com/kr/beanstalk"
      "os"
      "os/signal"
      "syscall"
      "time"
    )

    func main() {

      signalReceived := false

      //signal handling
      sigChan := make(chan os.Signal, 1)
      signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)
      go func() {
        <-sigChan
        signalReceived = true
        fmt.Println("==============\nreceived signal to shut down")
      }()

      beanstalkConn, err := beanstalk.Dial("tcp", "127.0.0.1:11300")
      fmt.Println(err)

      ts := beanstalk.NewTubeSet(beanstalkConn, "default")

      for !signalReceived {
        fmt.Println("trying to reserve a job..")
        jid, body, err := ts.Reserve(2 * time.Second)
        if err == nil {
          fmt.Println("working on", jid, string(body), err)
          beanstalkConn.Delete(jid)
        } else {
          fmt.Println("failed to reserve, probably time out")
          fmt.Println(err)
        }
      }

    }
