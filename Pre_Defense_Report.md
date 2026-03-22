# Дипломын урьдчилсан хамгаалалтын тайлан (Pre-Defense Report Draft)

Энэхүү баримт бичиг нь "дипломын урьдчилсан хамгаалалт"-д зориулан бэлтгэсэн тайлангийн ноорог бөгөөд төлөвлөгөө, системийн шаардлага, хийгдсэн ажлуудын архитектур болон кодын жишээнүүдийг багтаасан болно. Энэхүү текстийг `main.tex` болон холбогдох бүлгүүдэд хуулж оруулах боломжтой.

---

## 1. Ерөнхий мэдээлэл

*   **Сэдвийн Монгол нэр:** Сошиал мониторингийн систем дэх агуулгын шинжилгээ, сэдэв загварчлалын шийдэл
*   **Сэдвийн Англи нэр:** Content Analysis and Topic Modeling for Social Monitoring Systems
*   **Гүйцэтгэсэн:** П.Номин-Эрдэнэ (22b1num4640)
*   **Удирдагч:** Ж.Үйтүмэн

**Сэдвийн агуулга:** Энэхүү бакалаврын судалгааны ажил нь сошиал мониторингийн системд цугларсан их хэмжээний текстэн өгөгдөлд агуулгын шинжилгээ хийж, тухайн өгөгдлөөс сэдэв, түлхүүр ойлголт болон шинээр үүсч буй онцлох сэдвүүдийг автоматаар илрүүлэх сэдэв загварчлалд суурилсан шийдлийг зохион бүтээж, бодит системд хэрэгжүүлэхэд чиглэнэ.

---

## 2. Үечилсэн төлөвлөгөө (15 долоо хоног)

| Долоо хоног | Үе шат | Хийгдэх ажлууд |
| :--- | :--- | :--- |
| **1-2** | Судалгаа шинжилгээ | Сэдвийн хүрээнд онолын судалгаа хийх, Sumbee.mn системийн судалгаа хийх. |
| **3-5** | Системийн шинжилгээ, зохиомж | Системийн шаардлага тодорхойлох, архитектур болон өгөгдлийн сангийн зохиомж гаргах. |
| **6-9** | Системийн хөгжүүлэлт | Data Science модулийн хөгжүүлэлт (Topic Modeling, NER, Sentiment үнэлгээ). Backend API (FastAPI) болон Frontend (Next.js) хөгжүүлэх. |
| **10-13** | Тест хийж сайжруулах | Системд интеграци хийж нэмэлт feature хөгжүүлэх, бодит өгөгдөл дээр туршиж алдаа засах (жишээ нь: CORS, Ngrok тохиргоо). Урьдчилсан хамгаалалт (12 дахь долоо хоногт). |
| **14-15** | Тайлан бэлдэх | Шүүмж, алдаа засаж залруулах, эцсийн тайлан боловсруулалт, жинхэнэ хамгаалалт. |

---

## 3. Системийн шаардлага

Системийн шинжилгээний явцад дараах функциональ болон функциональ бус шаардлагуудыг тодорхойлсон.

### Функциональ шаардлагууд (Functional Requirements)
*   **Өгөгдөл оруулах:** Хэрэглэгч CSV файл эсвэл текст оруулан систем рүү илгээх боломжтой байх.
*   **Нэрлэсэн объект таних (NER):** Оруулсан текстээс хүн, байгууллага, байршлын нэрийг (Person, Org, Loc) автоматаар ялгаж таних.
*   **Сэтгэл хөдлөлийн шинжилгээ (Sentiment Analysis):** Тухайн нийтлэл эсвэл сэтгэгдэл эерэг, сөрөг, саармаг эсэхийг ангилах.
*   **Сэдэв илрүүлэх (Topic Modeling):** BERTopic загварыг ашиглан олон мянган өгөгдлөөс голлох сэдвүүдийг кластерлан гаргаж ирэх.
*   **Сүлжээний шинжилгээ (Network Graph):** Олдсон объектуудын хоорондын хамаарлыг (co-occurrence) граф хэлбэрээр дүрслэх.
*   **Мэдлэгийн сангийн удирдлага (Knowledge Base):** Админ хэрэглэгч зогсох үгс (stopwords) болон категориудыг удирдах боломжтой байх.

### Функциональ бус шаардлагууд (Non-Functional Requirements)
*   **Архитектур:** Hexagonal Architecture (Ports and Adapters) загварыг ашиглан NLP Core болон Web Framework-ийг салгаж, өргөтгөхөд хялбар байдлыг хангах.
*   **Хурд, гүйцэтгэл:** Backend хэсгийг FastAPI ашиглан хөгжүүлж API-ийн хариу өгөх хурдыг нэмэгдүүлэх; GPU дэмжлэгтэйгээр (Colab) инференс хугацааг 20ms хүртэл бууруулах.
*   **Интеграци:** Frontend болон Backend нь Ngrok болон REST API ашиглан найдвартай холбогдох (CORS тохиргоо хийгдсэн).
*   **Хэлний дэмжлэг:** Монгол хэлний морфологи болон дүрмийн онцлогт тохирсон Preprocessing хэрэгжүүлсэн байх.

---

## 4. Хийгдсэн ажлууд ба Системийн Архитектур

Одоогоор системийн бүх гол модулиудын хөгжүүлэлт болон интеграци хийгдээд байна. Систем нь 3 үндсэн бүрэлдэхүүн хэсгээс тогтоно:

1.  **Frontend (Next.js 16 + Tailwind CSS):** Хэрэглэгчийн интерфейс. Dashboard-д Recharts ашиглан Sentiment distribution, Network graph, болон Треемап (Treemap) зэрэг дүрслэлүүдийг харуулна.
2.  **API Adapter (FastAPI):** `adapters/api/routers` хэсэгт байрлах REST API цэгүүд. Frontend-ээс ирсэн хүсэлтийг хүлээн авч NLP Pipeline-рүү дамжуулна.
3.  **NLP Domain Core (Pure Python):** 
    *   *Preprocessing:* Монгол хэлэнд зориулсан текст цэвэрлэгээ, зогсох үгс хасах.
    *   *NER Engine:* Multilingual BERT загвар ашиглан нэрлэсэн объект таних.
    *   *Sentiment Analysis:* XLMR загвар ашиглах.
    *   *Topic Modeler:* BERTopic ашиглан кластер үүсгэх.
    *   *Network Analyzer:* NetworkX ашиглан объектуудын харилцааг тогтоох.

Саяхны гүйцэтгэсэн ажлууд дунд Backend талын CORS асуудлыг шийдвэрлэх, Google Colab дээрх backend-ийг локал frontend-тэй Ngrok ашиглан холбох, болон Topic Modeling-д зориулсан өгөгдлийн урьдчилсан боловсруулалтын (Preprocessing) сайжруулалт багтаж байна.

---

## 5. Хэрэгжүүлэлтийн код (Кодын жишээ)

Тайланд оруулахаар бэлтгэсэн чухал хэрэгжүүлэлтүүдийн кодын жишээнүүд:

### 5.1. FastAPI Router-ийн хэрэгжүүлэлт (API Endpoint)
Энэхүү код нь `adapters/api/routers/analysis.py` дотор байрлах бөгөөд хэрэглэгчээс датаг хүлээн авч NLP үйлдлүүдийг дууддаг.

```python
from fastapi import APIRouter, File, UploadFile
from nlp_core.preprocessing import preprocess_mongolian_text
from nlp_core.ner_engine import NEREngine
from nlp_core.sentiment import SentimentAnalyzer

router = APIRouter(prefix="/analysis", tags=["Analysis"])
ner_engine = NEREngine()
sentiment_analyzer = SentimentAnalyzer()

@router.post("/upload")
async def analyze_file(file: UploadFile = File(...)):
    # 1. Файлыг унших
    content = await file.read()
    text_data = content.decode("utf-8")
    
    # 2. Урьдчилсан боловсруулалт хийх
    cleaned_texts = [preprocess_mongolian_text(line) for line in text_data.split('\n') if line]
    
    # 3. NER болон Sentiment Prediction
    results = []
    for text in cleaned_texts:
        entities = ner_engine.extract_entities(text)
        sentiment = sentiment_analyzer.predict(text)
        results.append({
            "text": text,
            "entities": entities,
            "sentiment": sentiment
        })
        
    return {"status": "success", "results": results}
```

### 5.2. Монгол хэлний Preprocessing (Урьдчилсан боловсруулалт)
`nlp_core/preprocessing.py` доторх функцийг Topic Modeling болон бусад анализ хийхээс өмнө дуудаж текстийг цэвэрлэнэ.

```python
import pandas as pd
import re

def preprocess_mongolian_text(text: str) -> str:
    # 1. Хоосон эсэхийг шалгах
    if not isinstance(text, str) or not text.strip():
        return ""
        
    # 2. Текст жижиг үсэгт шилжүүлэх
    text = text.lower()
    
    # 3. Тусгай тэмдэгт болон тоог устгах
    text = re.sub(r'[^\w\s]', ' ', text)
    text = re.sub(r'\d+', '', text)
    
    # 4. Зогсох үгс (Stopwords) хасах (Жишээ)
    stopwords = {"байна", "болох", "тухай", "ба"}
    words = text.split()
    cleaned_words = [word for word in words if word not in stopwords]
    
    # 5. Илүүдэл зайг устгаж буцаах
    return " ".join(cleaned_words).strip()
```

---
*Энэхүү тайланг `Thesis/` фолдерт хадгалсан бөгөөд LaTeX `main.tex` рүү шаардлагатай хэсгүүд рүү хуулан ашиглана уу.*
