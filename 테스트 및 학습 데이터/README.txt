(깃허브에는 용량상 테스트 프로그램과 모델이 빠져있습니다. 해당 파일들은 http://naver.me/xorV7cZr, http://naver.me/Gtt54nnt에서 다운로드 가능합니다.)

1. table_data에는 수집하고 전처리한 html형식의 스펙표가 있고, qa_dataset.xls은 수집한 질의응답 데이터셋입니다.
모델 폴더 내에는 파인튜닝 전 모델과 파인튜닝 후 모델이 있습니다. 파인튜닝 후 모델은 발표자료 24p의 A,B번 모델이고 file_tuned_with가 'KorQuAD 2.0 와 스펙 표 질의응답 데이터를 함께 파인 튜닝', trained_on이 '스펙 표 질의응답 데이터 로 파인 튜닝', trained_with가 'KorQuAD 2.0 로 파인 튜 닝 후 스펙 표 질의응답 데이터 로 파인 튜닝'에 해당됩니다.


2. 테스트를 하기 위해서는 우선 다음과 같은 프로그램과 라이브러리들을 설치해야 합니다.
1) anaconda를 설치해야 합니다.
2) 가상환경을 만들고, 아래의 라이브러리들을 설치해야 합니다.
-python 3.6
-beautifulsoup4
-eunjeon
-h5py
-numpy 1.19.5
-tensorflow-gpu 1.13.1
-transformers

3. 아래의 파일 내에있는 주석을 참고하여 실행환경에 맞게 파일 수정이 필요합니다.

TAPAS_with_TMN.py

1) 183, 184 line에서 모델이 저장된 경로를 변경
2) 1461 line의 range문의 숫자를 (get_factory_table_dataset_정답여러개.py에서 저장된)질의응답 개수에 맞게 테스트 횟수 변경

get_factory_table_dataset_정답여러개.py

1) 26번 line에서 xls파일의 경로를 변경
2) 60번 line에서 html형식의 스펙표가 있는 경로를 변경

get_factory_table_train_dataset_정답여러개.py
1) 23번 line에서 xls파일의 경로를 변경
2) 55번 line에서 html형식의 스펙표가 있는 경로를 변경


4. 테스트를 진행하는 순서는 get_factory_table_train_dataset_정답여러개.py -> get_factory_table_dataset_정답여러개.py -> main3.py 순입니다.

get_factory_table_train_dataset_정답여러개.py은 질의응답 데이터셋이 테스트 가능한지 확인하고,
get_factory_table_dataset_정답여러개.py 파일은 테스트 가능하다고 판단된 질의응답 데이터에 대해서 모델이 빠르게 테스트할 수 있게 npy파일로 만들고, 
이를 main3.py에서 TAPAS_with_TMN의 테스트 함수를 불러와서 테스트를 진행합니다.
테스트 결과는 터미널 창에 출력되며, 이를 가시성 있게 엑셀 파일로 다시 나타내는 분석자동화.py파일도 있습니다. 
분석자동화 파일을 사용하려면 분석자동화.py파일의 9번 line 주석을 참고하면 되겠습니다.
분석자동화.py 코드에 대한 설명은 https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/%EC%97%B0%EC%8A%B5%EC%BD%94%EB%93%9C/QA%20%EC%8B%A4%ED%97%98%EA%B2%B0%EA%B3%BC%20%EB%B6%84%EC%84%9D/QA_%EC%8B%A4%ED%97%98%EA%B2%B0%EA%B3%BC%20%EB%B6%84%EC%84%9D%20%EC%9E%90%EB%8F%99%ED%99%94%20%EC%BD%94%EB%93%9C.md에 나와있습니다.
또, 번거롭게 여러번 py파일을 실행하는 대신, 한번에 실행하도록 도와주는 main4.py파일도 있습니다.

추가로 수집한 html형식의 스펙 표를 전처리하는 코드와 설명도 https://github.com/lukaskorea/Graduateproject_Teamscikitlove/blob/main/%EC%97%B0%EC%8A%B5%EC%BD%94%EB%93%9C/html%20%EC%A0%84%EC%B2%98%EB%A6%AC/Samsung%20%EC%A0%84%EC%B2%98%EB%A6%AC%EC%BD%94%EB%93%9C.md에 나와있습니다.
