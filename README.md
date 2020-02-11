# address
国家行政机构代码
package main

import (
   "encoding/json"
    "fmt"
   "strings"
   "os"
)

type Village struct {
    Code   string
    Name   string
}
type Areas struct{
    Code string
    Name string
    Children []Village
}
type City struct{
    Code string
    Name string
    Children []Areas
}
type Province struct{
    Code string
    Name string
    Children []City
}
type Address struct{
    ProvinceCode string
    CityCode     string 
    AreasCode    string
    Detail       string
}

func UnicodeIndex(str,substr string) int {
  result := strings.Index(str,substr)  
  if result >= 0 {
    prefix := []byte(str)[0:result]  
    rs := []rune(string(prefix))  
    result = len(rs)
  }
  
  return result
}

func main() {
   
    fmt.Println("请输入地址")
    var address  string
    var P,C,A  string
    var p,c,a,v     int
    var rs     []rune
    var AD  Address
    fmt.Scanln(&address)
    p= UnicodeIndex(address, "省")  
    c=UnicodeIndex(address,"市")
    a=UnicodeIndex(address,"区")
    rs = []rune(address)
    v=len(rs)
    P=string(rs[0:p+1])
    C=string(rs[p+1:c+1])
    A=string(rs[c+1:a+1])
    AD.Detail=string(rs[a+1:v])
    filePtr, err := os.Open("./pcas-code.json")
    if err != nil {
        fmt.Println("文件打开失败 [Err:%s]", err.Error())
        return
    }
    defer filePtr.Close()
    var info []Province
    decoder := json.NewDecoder(filePtr)
    err = decoder.Decode(&info)
    if err != nil {
        fmt.Println("解码失败", err.Error())
    } else {
    }
    for _, v:= range info {
    if v.Name==P{
        AD.ProvinceCode=v.Code
        //fmt.Println(PC)
        for _,v1:=range v.Children{
        if v1.Name==C{
        AD.CityCode=v1.Code
            //fmt.Println(CC)
       for _,v2:=range v1.Children{
            if v2.Name==A{
                AD.AreasCode=v2.Code
                //fmt.Println(AC)
            }
        }
        
        }
    }
    }

}
fmt.Println(AD)
}
