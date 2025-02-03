## 1주차 과제 회고
### 함수로 최대한 나눠서 만들어보자
  본 과제에서 가장 공을 들였던 부분은 함수를 사용해서 가독성을 높이고 간단하게 만들어 보는 것을 중점으로 두었다. 본 과제의 main() 부분은 아래와 같다.
  ```python
    def main():
        igp = None
        start()
  ```
  start() 부분을 살펴봐도 최대한 간단하게 가독성 있게 작동하려고 노력했다.
  ```python
  def start():
   print("...")
   sel_menu = input("1, 2, 3 중 하나만 선택해주세요.")
   while True:
    if sel_menu == "1":
      clear()
      select_char()
      break
    elif sel_menu == "2":
      clear()
      create_char()
      break
    elif sel_menu == "3":
      break
    else:
      print("1,2,3 중에 선택해주세요.")
      sel_menu = input()
  ```
    해당 부분을 보면 알 수 있듯이 1번을 선택하면 select_char()로 2번을 선택하면 create_char()로 이동하도록 코딩하였다.

### 디버깅
 ```select_char()```부분에서 에러가 발생하였다.</br>
  작동은 잘 되나 모든 행동을 끝 마치면 종료가 되야하는데 ```select_char()```로 돌아가는 문제가 발견되었다. ```while```문에서 잘못된 줄 알고 한참을 찾았었는데, 문제는 ```else```를 빼먹어서 코드가 실행될 부분이 남아있기 때문에 발생되었다. 코드는 아래와 같다.
  ```python
  if len(characters) == 0:
    # characters가 없으면 캐릭터를 만들어야 한다고 만들었다.
    create_char()
    (...) # 이하 코드 생략

  print("-------------------------------")
  print("캐릭터를 선택해주세요.")
  (...) # 이하 코드 생략
  ```

  1주차 과제에서는 캐릭터 생성을 하면 다시 ```start()``` 함수를 불러왔다.</br>
  그래서 다시 캐릭터를 선택하도록 만들고 다음에는 게임이 진행되도록 하였는데, 위의 코드를 보면 알 수 있듯이 캐릭터가 없는 상태에서 캐릭터 선택을 만들면 ```if```문 안으로 들어가 캐릭터를 생성하게 되고 ```start()```로 들어가 게임이 진행되게 된다.</br>
  만약 모든 행동을 마치면 ```if```문 다음에 있는 ```print(...)```부터 차례로 실행되게 된다. 즉, ```else```를 빠뜨렸기 때문에 하지 않아도 될 행동이 남아 있는 것이다.</br>
  누구나 한 번에 코드를 깔끔하게 짜는 것은 아니지만 이렇게 사소한 실수는 머리 속으로는 구상을 해보면 아무 문제가 없다고 생각하게 되어 오류를 찾는게 더욱 힘들었다. 앞으로는 이런 실수는 하지 않도록 해야겠다.

### CLI내용 clear하기
해당 내용은 같은 팀원 victoria의 조언으로 해결할 수 있었다.
- 기존에 알고 있는 ```os.system('cls')```는 Windows에서 작동하는 명령어다 따라서 그전에 OS type을 먼저 확인할 필요가 있다.</br>
  os를 확인은 아래와 같이 확인할 수 있다.
  ```python
  import platform

  ostype = platform.system()
  ```
  ostype의 return 값은 아래와 같다.
  - Windows : "Windows"
  - macOS : "Darwin"
  - Linux : "Linux"</br>
  
- 따라서 함수를 다시 작성하면 아래와 같은 예시로 작성할 수 있다.
  ```python
  import platform

  def clear():
    ostype = platform.system()

    if ostype == "Linux" or ostype == "Darwin":
      os.system('clear')
    elif ostype == "Windws"
      os.system('cls')
  ```

### Class의 이름의 첫 글자는 대문자로
해당 내용은 Alex의 과제 피드백으로 다시 공부할 수 있었다.
- 코드 컨벤션 or 네이밍 컨벤션이라고도 하며 일반적인 프로그래밍 언어에서는 class 이름을 대문자로 시작하는 것을 권장하며 이를 **PascalCase**라고 한다.</br>
매서드나 변수는 일반적으로 소문자로 시작하는 **camelCase**를 사용한다.

- 이런 컨벤션, 규칙이 있는 이유는 코드의 가독성과 구분성을 향상시키기 위함이다. 따라서 1주차 과제에서 Class 네이밍을 수정하도록 하자.

[1주차 과제 순서도](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=%E1%84%8C%E1%85%A6%E1%84%86%E1%85%A9%E1%86%A8%20%E1%84%8B%E1%85%A5%E1%86%B9%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%83%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%A5%E1%84%80%E1%85%B3%E1%84%85%E1%85%A2%E1%86%B7.drawio#R%3Cmxfile%3E%3Cdiagram%20id%3D%22C5RBs43oDa-KdzZeNtuy%22%20name%3D%22Page-1%22%3E7V3Zdpu6Gn6aXMZLA%2BNlmmn3nLQ7bbo7XHURm9icYpONceL06Q%2FYCKMhgG2BBGkusozAGPRP3z9JJ%2Fh8vr6OvcfZh2jihycITNYn%2BOIEIcc00v%2FZwAsZMLcD0ziYbIfgbuAu%2BO3ngyAfXQUTf0ldmERRmASP9OA4Wiz8cUKNeXEcPdOXPUQh%2FauP3tTnBu7GXsiPfgsmySx%2FC2Tvxv%2Fyg%2BmM%2FDK03O2ZuUcuzt9kOfMm0XNpCF%2Be4PM4ipLtp%2Fn63A%2BzuSPz8u39y7fw5pd1%2FZ9Py3%2B9f97998vHr6fbm13t85XiFWJ%2FkRx864fbxfordOb46dv7yfLm%2Ffyv8fMpzKfhyQtX%2BYTlL5u8kBn0J%2BmE5odRnMyiabTwwsvd6Ls4Wi0mfvY7ID3aXXMTRY%2FpIEwH%2F%2BcnyUvOHd4qidKhWTIP87MNXzCfiGW0isd%2BxVvlFEy8eOonVW9vbS%2FM3rDELfn8XfvR3E%2Fil%2FSC2A%2B9JHiiWcrLOXNaXLeb%2FfRDToA9iOFytLhLXyHhCLJM4uhXwc2InsqUSx%2Bz6%2BbraSbPo4cweh7P0tuMltnNfmaXP8%2BCxL979DaT%2BJxetjcJnvw48deVc0bOgpzFcv2Bc2l63gkjBPnYrCSI5Drpswz5aR4AyxNWrud5RxXPA%2B%2FuwvkxQ3dX7vfrr%2Fc%2F7U93v04Nh6MGFpLjxrtPDRM1hV4YTBfp53E6hX6cDmRcGaSq%2Fyw%2FMQ8mky21%2FGXw27vf3C%2Bj12MULJLNq5jvTswLITmqmYfl%2F8KA5b9C2QiRXJyCEQJuznqN5zq%2F3W32%2FKV7Yfob0cPD0k842hQPcbjwIPNNC48BZQuPmKDIBJTShACPbAO62DQc7NjAdeg7bh88v8lRVBcLKY8SxGTXQ0gLJj1aSMHIBMR%2BHUrS9oXScFXIoL8Oku%2Blzz%2ByW43M%2FOhind95c%2FBCDhbpm34vH5S%2BlR3uvrY5It9TJu9IL2NpcXIIhaTXQw4NacYy5RDbcXSXQ8iTZ%2B4vVhyF4lk0v18tXwHiZSzPAf39BGEPlA5plO7yKN0RgHQSC5AP0nlYeLmY8LwehsHj0tdpJh16Ht1G84jtUVszSSBLaSbPY99L%2FHTsPPULvXGmDjgWLawDrJ9c734ZhavEP4vHubXYjO6OjE4pwIInAS8LPU6UYayWqEAeaVi4mZjHejtq62VHDZWAaQeSfpTO1AGmkWVhGjS5wK2BTZujWz8O0mnLbH4bWEo%2BRBK7RNABI4Dd3Z9FCTlEzggARom%2B4hZx98aQcbcMnAL%2F3U85jLu1nRTuvmdx7L2ULsuB06uvhAHzs5ZV%2BZjQqbw%2B%2FbB9Arm%2BH%2B%2Fxk3i4YsxZKdpSAjTAdE1qxolU6ItBiTiWg8h%2BmCVW3oq5h9giYKrO4pNonnxzL3bL2jYwxFjAE5XeNTH2qqJp%2B6phw6CFHAIkVa1WTlJJTr%2FMMkz%2BcTW%2FzyQURA9lkU2fP2Vgy5tnwhgmG6oBnssOTA1N%2FHGwDKJFx7khzBo04j2VBReL%2FCWrLcFVAsvFyLAAg9XIcE9h7wQTkkR%2FnQZwZSuA44jPQ50f%2FpJjiJS5k%2FZwTsYdy51Z3Qr0eRRGGa0W0SK7y0MQhsxQeyJKB4ZsPqBhdJm9NZXGusHJHrHu9ICVs5I7Z5bFFtYIrWT5NJtaaM0KJCwC7rXzRSp5VYovgm0SSyS%2BSG6qDvVFyG0YU9qea2LxrokKWVYlS5Z0tHucIuUBaMk%2FtDYQ8z77NM0%2BER%2By13CT8xSdhpFhsy1rZonVl6Z%2BYifQsak4mdJDylKcR5THIFt1HkmJ6rA0qdXUa7BMrTQpee4SNW5D74WjiDYpygKxE561BHoQdYnqLcxN4ceIm8C35Xi5jLGy%2BdhIp56XLa7L1DFptofnBWpsnmwdZzbUcTZQpeOEaRUeuutceERYVUbhEXRtJjx8cpTb1b6fZfcnu62%2FoLae1raY9gjI6O%2BWS3t57CKGkpoItrTsLhhhgHom2BYfnz4bJ5kf3W%2B%2FnHHLi6CeOrfcUaJBd5oPug7tmltuXYi4C%2Be8MXpRFzcWKjm%2B%2BE7nHiPCfVLCxqbBJHC013K2mtqJAzI8kqWLQH4F8Y%2BDQl%2BIVtsYUZGvusuR0UGgzOYl%2Fy7xkhWf1O1zBRSma84LCFmyn2ZLkQKhuuWDOR%2BixXKTVwCffaI2hjL7bOsEFkXT2upwFjvrarIKvXUAxY6RwWtj8YXKiljExOdz5CSd9xBt9PGOLax%2FVxE5cbqNfZ6lFziP6818ktMkCbimC9Cy%2BtgTdL6RBHJiuj2Rlc1aeZXa1fZnt7fQA3ZVSo0M39JwMKKUwrHJeqmwS%2Fz6amHXW1Maeq3CIGzrl6M0wAibTOEqvlhzI6mL20ed8RpaOKzTnC7w0V9liEOFf1RGOypDrx66V0Iof4jfDvH1CqghvtN9X3thv2ovQK29ACO4BZ4p0qTPiAxLL8GoxAymZRj6mxLROnRyAIgfLv23zACnqTuCMaSghU5B4ErFSLUxevF4FiymHNX6le1i40WGIFzXbbwImr0x5QeabcnJsaqoZ70pN%2FQy5WpynUctHtVV80wlFFNQ8C9OrGDMJGKMjktIeDB45QUhx1Z9DvIjm86xGILWAVE1JmpNZ%2FOA6W%2Fv16DmHDKLL6ifc35RqrswNSmDmnVXN0bnex3SyQkjHgb2edY143SsBhHqEZTBTZEc1grIYT7t%2Fvn2elBSwqXalbtOJLLSA9dJ3ygoFvT8iWdbWQvt5Dn4%2B90niD7%2FvLicQHDz%2FNu8ENS5KCwrbBwoarKIFqoUQzCygNyqQpIlb2Mdc%2BEbqlmgSZncKCtREcoNH9tT2EwkU27MavOVVeMarcgNs%2B5hi3Kjxt71zE6RaktN5I0v7hBTsX%2FyVmenTAj17fGp5LGh9fiw0N00G0J3GU0%2B4plWGvgeStYDNtWJWK%2FyFax2HbGhUN%2FpJ%2FVJMO0P9Y%2BivttT6vNNz510BbndhpTprKBl8AaXmMBuQmVqCgYP3HFIKD%2FdNglW8m7fZM4UtaX0PgJjNNWApl65gmJnppKXkSTeeGAJZMbnEKlAsoxcJyrQFJXN9l4IiKGpFwJl67ZVPnc5Ybbi%2Few%2BSwACmkmANUwJkK7dxZVe0DBHdDzLthhKtVzr5fBm%2FEu84itg9CmkLjhOymoaNjpyF0K6t6uDQmq1zeCH1XfuB9VbEuZac%2BZKj%2FKLpR4xu1M5Des791%2FPA9G6Jf%2Fd1xf0QMLnanU7Koh45%2F3K27R4aKuCChmUsh6hAQxq2rVPaZh8nQRxNthlxs%2BjxUSY7Gi8ZSqN1tS5HszWbpZgE0pXkO1gRVsa8LKV4KxS9NJxrLJahyMAkQYxTLPpkmadVfMzeyIZoB1tjw16A0VG27elvXuHH4ngyNnEAxI26kuPv6s4aqu04EOkHDqCgsfVzAvW89AdJLkSO57RsV5aB6CIp9Am%2BDRYRJR6CyNmJW0RKLKKCEc3sIhv6VGh7yTrLburxasZDOGwDSktR6NsXoiynTc2Cw0OMZ%2FBRHNtwUqDdpcpXZuP5u5WehwgAQq7QvjdtdUSoOCINwnPyF6U9S33QK%2FlcyCxMv1xgwpOk%2BIHWQaSGsQid2ZWOemsLB4KWoOHgCOaCph2%2Fg8fctTd%2F9mxkJRElck0mmgfaCC8JsISrFN04Xupue27R8TGiAv0rCpG7PKplQEoMTKFtUrMVgYShF07PDHUQgSZjTtutWSkCgyZJo219V9BmTBawzxXMIywDtER2igxS4XOKjyi4uCIMmPZClCwyKgwxSW9S1iM0k3MZrjoO0hKcJnMhiMuROXb1V4veX%2BSV7yq3jmCrsx6Khv0Lh%2FGJzBJyHGYKBVbjIJ3kWIFrzrkBSxEK3mj683ZKuFrvbOu1wKUrtrN9npisDvq8TnI0hpMXsK0OzGd%2FSsEdGWGeAzGdBL20td2Qsh7SCpkX7K4FtmFBnkIZQvxVD95iSDXGe3B5fdbHrn0OHNXvClxBsj%2BTMoyd5APUQ9CFhrn5HJ3TBtZECxjrLkrtuMhGfYEmqQTTGcLonRv%2B4O6WfYEmF0IpNICZ4NZmKDpcuV7o1Kb%2Fp0UMFb3s7BfQJ3AWNi%2FUs1CCuXEgDCSukYXuTNbi9aiViIyVqLhBkABel%2FMzRnwwVu%2FBrB6Fhqy2W5UV1DQCUULV7cWHCpa%2FvsRTZCt7Amm0kfZG4yyb6mbhVf2tPLWRdkjTlHc%2BE9%2B9sV%2Fhu1wpcfdFasK8%2Bn964OWF5pkxFxFdcU2Vtl1aBIzYs70KNdeLzuUKeRMXikoRoAHlnq8LnZV%2BM8yiRurQSAzPYyjbPvA3eXZ%2Bqkfokk265f%2FBw%3D%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E)
[1주차 과제 colab](https://colab.research.google.com/drive/14_YKxuLfIH_hAM9zfgGW1DsBOX6djTv4?usp=sharing)
[1주차 과제 GitHub](https://github.com/thplus/brix-assignment/blob/main/1st/1st_assign.py)