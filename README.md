import xml.etree.ElementTree as ET
import pandas as pd

def xml_to_csv(xml_file, csv_file):
    # XML 파일을 UTF-8 인코딩으로 파싱
    with open(xml_file, 'r', encoding='utf-8') as file:
        tree = ET.parse(file)
    root = tree.getroot()
    
    # 모든 데이터를 저장할 리스트
    data = []
    
    # XML 구조에 맞게 데이터를 추출
    for sentence in root.findall('.//sentence'):
        for word in sentence.findall('word'):
            row = {
                'sentence_id': sentence.get('id'),
                'document_id': sentence.get('document_id'),
                'subdoc': sentence.get('subdoc'),
                'word_id': word.get('id'),
                'form': word.get('form'),
                'lemma': word.get('lemma'),
                'postag': word.get('postag'),
                'head': word.get('head'),
                'relation': word.get('relation')
            }
            data.append(row)
    
    # 데이터가 있는지 확인
    if not data:
        print("No data found in the XML file.")
        return
    
    # 데이터를 DataFrame으로 변환
    df = pd.DataFrame(data)
    
    # DataFrame을 CSV 파일로 저장
    df.to_csv(csv_file, index=False, encoding='utf-8-sig')
    print(f"Data successfully written to {csv_file}")

# 파일 경로 설정
xml_file = r'C:\My python\glaux-trees-master\glaux-trees-master\public\xml\0526-004.xml'
csv_file = r'C:\My python\glaux-trees-master\glaux-trees-master\public\xml\0526-004.csv'

# 함수 호출
xml_to_csv(xml_file, csv_file)
