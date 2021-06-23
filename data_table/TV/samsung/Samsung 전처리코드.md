### 삼성전자 공식홈페이지 스펙 표 전처리 코드

        <div class="spec-table">
            <dl>
                <dt>디스플레이</dt>
                    <dd>
                        <ol>
                            <li>
                                    <strong class="spec-title">화면크기</strong>
                                        <p class="spec-desc">214&nbsp;cm</p>
                            </li>
                            <li>
                                    <strong class="spec-title">해상도</strong>
                                        <p class="spec-desc">3,840 x 2,160&nbsp;</p>
                            </li>
                        </ol>
                    </dd>
            </dl>
        </div>

이런 형태의 html 파일 에서 div 를 table로, dl을 tr로, dt를 td로, ol, dd 는 삭제하고 li는 td로 변경, css를 넣어주기

            <head>
            <style>

            table {
                font-family: arial, sans-serif;
                border-collapse: collapse;
                width: 100%;
            }

            td, th {
                border: 1px solid #dddddd;
                text-align: left;
                padding: 8px;
            }

            </style>
            </head>

            <table>
                <tr>
                    <td>디스플레이</td>
                    <td>
                        <strong class="spec-title">화면크기</strong>
                        <p class="spec-desc">214&nbsp;cm</p>
                    </td>
                    <td>
                        <strong class="spec-title">해상도</strong>
                        <p class="spec-desc">3,840 x 2,160&nbsp;</p>
                    </td>
                </tr>
            </table>

###### 파이썬 코드

        pip install lxml

        import pandas as pd
        import re
        import codecs
        import os

        print("before: %s"%os.getcwd())
        os.chdir("C:/Users/user/Desktop/html")
        print("after: %s"%os.getcwd())

        for filename in os.listdir(os.getcwd()):
        if filename[-4:] == 'html':

            file_before = codecs.open(filename, "r", "utf-8")
            file_after =  file_before.read()

            file_after = re.sub('<div class="spec-table">', '<table>', file_after)
            file_after = re.sub('</div">', '</table>', file_after)

            file_after = re.sub('<dl>', '<tr>', file_after)
            file_after = re.sub('</dl>', '</tr>', file_after)

            file_after = re.sub('<dt>', '<td>', file_after)
            file_after = re.sub('</dt>', '</td>', file_after)

            file_after = re.sub('<dd>', '', file_after)
            file_after = re.sub('</dd>', '', file_after)

            file_after = re.sub('<ol>', '', file_after)
            file_after = re.sub('</ol>', '', file_after)

            file_after = re.sub('<li>', '<td>', file_after)
            file_after = re.sub('</li>', '</td>', file_after)

            file_after = '''
            <head>
            <style>

            table {
                font-family: arial, sans-serif;
                border-collapse: collapse;
                width: 100%;
            }

            td, th {
                border: 1px solid #dddddd;
                text-align: left;
                padding: 8px;
            }

            </style>
            </head>
            ''' + file_after


            html = file_after

            print("------------------------")
            print("before: %s"%os.getcwd())
            os.chdir("C:/Users/user/Desktop/html/html_after")
            print("after: %s"%os.getcwd())
            locate = (
            "C:\\Users\\user\\Desktop\\html\\html_after/"
                + filename
            )

            # Write HTML String to filename.html
            with open(locate, "w") as file:
                file.write(html)
                print(filename+'생성완료')
            print(os.listdir(os.getcwd()))
            print("------------------------")
            print("before: %s"%os.getcwd())
            os.chdir("C:/Users/user/Desktop/html")
            print("after: %s"%os.getcwd())

        else:
            print(filename+'은 html 파일이 아님')

원본


<img src="https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/data_table/TV/samsung/samsung_tv.png?raw=true" height="750" width="750">

전처리전 html 파일
![삼성 tv 스펙 html 표(전처리전) 레이아웃](https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/data_table/TV/samsung/samsung_tv_html.png?raw=true)

전처리후 html 파일
![삼성 tv 스펙 html 표(전처리후) 레이아웃](https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/data_table/TV/samsung/samsung_tv_html_after.png?raw=true)