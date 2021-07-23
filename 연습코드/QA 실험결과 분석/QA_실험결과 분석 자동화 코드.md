### QA_실험결과 분석 자동화 코드

        -----
        1
        The code that caused this warning is on line 16 of the file E:\pycharm2\KorQuAD2_table_koelctra\evaluate2.py. To get rid of this warning, pass the additional argument 'features="html.parser"' to the BeautifulSoup constructor.

        return BeautifulSoup(t).get_text()
        score: 0.9071135520935059 None <class 'type'> F1: 0.6666666666666666  ,   3500U
        question: "ASUS ZenBook 13 - UM433DA는 CPU 넘버는 무엇입니까?"
        answer: 3500U (2.1GHz) ,
        EM: 0.0
        F1: 0.7083333333333333
        -----
        2
        score: 0.607709527015686 None <class 'type'> F1: 0.2222222222222222  ,   와이드 16 : 9
        question: "ASUS ZenBook 13 - UM433DA의 디스플레이 해상도는 몆인가요?"
        answer: 1920x1080(FHD) ,
        EM: 0.0
        F1: 0.5462962962962963

이런 형태의 txt파일에서 qustion, answer, 모델이 추론한 정답 값 저장하고 EM값 비교를 통해서 모델이 정답을 맞췄는지 기록

        QA_list = [['질문','답변','모델이 추론해낸 응답','정답 여부', '질문 유형']] 
        f = open("QA_실험결과 위치", 'rt', encoding="utf-8")
        prev_EM = 0
        #똑같은 형식의 QA_실험결과 하드코딩으로 질문, 정답, 모델이 준 응답, 정답 여부 값 찾아서 넣기 
        # 추가로 질문 유형은 pd파일에서 검색 기능을 통해 찾아보기
        while True:
            
            line = f.readline()
            if line.find('score') >-1:
                
                model_ans = line[line.find('F1: ')+len('F1: ,'):]
                model_ans = line[line.find('   ')+len('   '):]
                line = f.readline()
                
                question = line[line.find('question: ')+len('question: '):-1]
                question = question.replace('"', '')
                question = question.lstrip()#첫 부분에 공백이 오는 경우 제거
                line = f.readline()
                
                ans = line[line.find('answer: ')+len('answer: '):-2].lstrip()
                ans = ans.replace('\n',"")
                line = f.readline()
                
                cur_EM = float(line[line.find('EM:')+len('EM: '):])
                if cur_EM > prev_EM:
                    result = '맞음'
                else:
                    result = '틀림'
                
                prev_EM = cur_EM
                QA_list.append([question, ans, model_ans, result, '못 찾음'])
            
            if not line:
                break
            
        f.close()

![출력 결과](https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/%EC%97%B0%EC%8A%B5%EC%BD%94%EB%93%9C/QA%20%EC%8B%A4%ED%97%98%EA%B2%B0%EA%B3%BC%20%EB%B6%84%EC%84%9D/QA_Result_1.PNG?raw=true)

추가로 현재 질문이 어떤 유형인지 알아내기 위해서 질의응답 데이터셋을 pandas를 이용해서 검색해서 찾아냄
        import pandas as pd
        import xlrd

        df = pd.read_excel('C:/Users/user/Desktop/qa_dataset.xlsx')
        print(df)

        #질문 유형 pandas dataframe을 통해 찾아보기
        for i in range(len(QA_list)):
            question = QA_list[i][0]
            try:
                row = df[df['질문'] == question]
                question_type = row['질문유형'].values[0]
                QA_list[i][4] = question_type
            except:
                try:
                    #ans = ans.lstrip()
                    question = '"'+question+'"'                     #검색이 안되는 경우 질문 양쪽에" 붙여서 다시 검색
                    row = df[df['질문'] == question]
                    ans_type = row['질문유형'].values[0]
                    QA_list[i][4] = ans_type
                except:
                    print(f'{question}는 검색 실패')

![출력 결과](https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/%EC%97%B0%EC%8A%B5%EC%BD%94%EB%93%9C/QA%20%EC%8B%A4%ED%97%98%EA%B2%B0%EA%B3%BC%20%EB%B6%84%EC%84%9D/QA_Result_2.PNG?raw=true)

질문별로 알아낸 질문, 답변, 모델이 추론한 응답, 정답 여부, 질문 유형을 엑셀파일로 쓰기

        import openpyxl

        wb = openpyxl.Workbook()
        sheet = wb.active

        for i in range(len(QA_list)):
            sheet.append(QA_list[i])

        wb.save('QA_실험결과_분석.xlsx')

| 질문                                          | 답변             | 모델이 추론해낸 응답    | 정답 여부 | 질문 유형 |
| ------------------------------------------- | -------------- | -------------- | ----- | ----- |
| ASUS ZenBook 13 - UM433DA는 CPU 넘버는 무엇입니까?   | 3500U (2.1GHz) | 3500U<br>      | 틀림    | 어휘 포함 |
| ASUS ZenBook 13 - UM433DA의 디스플레이 해상도는 몆인가요? | 1920x1080(FHD) | 와이드 16 : 9<br> | 틀림    | 어휘 포함 |
| ASUS ZenBook 13 - UM433DA의 메모리는 몆기가에요?      | 8GB            | 8GB<br>        | 맞음    | 어휘 포함 |
| ASUS ZenBook 13 - UM433DA는 그래픽칩셋을 뭘 사용합니까?  | RX Vega 8      | RX Vega 8<br>  | 맞음    | 어휘 변형 |
| ZenBook 13 OLED-UX325EA는 어떤 색상을 가지고 있나요?    | Pine Grey      | Color<br>      | 틀림    | 어휘 변형 |

