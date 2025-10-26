<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>맞춤 여행지 추천</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 600px;
            width: 100%;
            padding: 40px;
            animation: fadeIn 0.5s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h1 {
            color: #667eea;
            text-align: center;
            margin-bottom: 10px;
            font-size: 2em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 0.9em;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: #e0e0e0;
            border-radius: 10px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
            transition: width 0.3s ease;
        }

        .question-container {
            display: none;
        }

        .question-container.active {
            display: block;
            animation: slideIn 0.4s;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .question {
            font-size: 1.3em;
            color: #333;
            margin-bottom: 25px;
            font-weight: 600;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            background: white;
            font-size: 1em;
        }

        .option:hover {
            border-color: #667eea;
            background: #f8f9ff;
            transform: translateX(5px);
        }

        .option.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 30px;
        }

        button {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .btn-prev {
            background: #e0e0e0;
            color: #666;
        }

        .btn-prev:hover {
            background: #d0d0d0;
        }

        .btn-next {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-next:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-next:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        .result-container {
            display: none;
        }

        .result-container.active {
            display: block;
            animation: fadeIn 0.5s;
        }

        .destination {
            background: #f8f9ff;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            border-left: 4px solid #667eea;
        }

        .destination h3 {
            color: #667eea;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
            justify-content: space-between;
        }

        .destination .title-section {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .destination .country-name {
            font-size: 1.5em;
        }

        .destination .flag {
            font-size: 2em;
        }

        .destination .match-score {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9em;
            font-weight: 600;
        }

        .destination p {
            color: #666;
            line-height: 1.6;
            margin-bottom: 10px;
        }

        .tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
        }

        .tag {
            background: #667eea;
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.85em;
        }

        .restart-btn {
            width: 100%;
            margin-top: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .select-btn {
            width: 100%;
            margin-top: 15px;
            padding: 12px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .select-btn:hover {
            background: #5568d3;
            transform: translateY(-2px);
        }

        .destination.selected-destination {
            border-left: 6px solid #667eea;
            background: #e8ebff;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.2);
        }

        .final-choice {
            display: none;
        }

        .final-choice.active {
            display: block;
            animation: fadeIn 0.5s;
        }

        .final-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 40px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 20px;
        }

        .final-card .flag {
            font-size: 5em;
            margin-bottom: 15px;
        }

        .final-card h2 {
            font-size: 2.8em;
            margin-bottom: 15px;
        }

        .final-card .congrats {
            font-size: 1.3em;
            opacity: 0.95;
            margin-bottom: 10px;
        }

        .final-details {
            background: white;
            padding: 25px;
            border-radius: 10px;
            margin-top: 20px;
            color: #333;
        }

        .final-details h3 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.4em;
        }

        .final-details p {
            line-height: 1.8;
            margin-bottom: 15px;
        }

        .map-container {
            background: white;
            padding: 25px;
            border-radius: 10px;
            margin-top: 20px;
        }

        .map-container h3 {
            color: #667eea;
            margin-bottom: 15px;
            font-size: 1.4em;
        }

        .map-frame {
            width: 100%;
            height: 400px;
            border: none;
            border-radius: 10px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        @media (max-width: 480px) {
            .map-frame {
                height: 300px;
            }
        }

        .start-screen {
            text-align: center;
        }

        .start-screen h1 {
            font-size: 2.5em;
            margin-bottom: 20px;
        }

        .start-screen p {
            font-size: 1.1em;
            color: #666;
            margin-bottom: 30px;
            line-height: 1.8;
        }

        .start-btn {
            width: 100%;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            font-size: 1.2em;
        }

        @media (max-width: 480px) {
            .container {
                padding: 25px;
            }

            h1 {
                font-size: 1.5em;
            }

            .question {
                font-size: 1.1em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 시작 화면 -->
        <div id="startScreen" class="start-screen">
            <h1>🌍 나만의 여행지 찾기</h1>
            <p>
                AI가 여러분에게 딱 맞는 여행지를 찾아드립니다.<br>
                여러분의 여행 성향을 기반으로 전 세계 국가 중<br>
                최적의 여행지를 추천해드려요!
            </p>
            <button class="start-btn" onclick="startQuiz()">시작하기</button>
        </div>

        <!-- 퀴즈 화면 -->
        <div id="quizScreen" style="display: none;">
            <h1>여행지 추천</h1>
            <div class="subtitle">질문에 답하고 맞춤 여행지를 찾아보세요</div>
            
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>

            <!-- 질문들 -->
            <div id="questionsContainer"></div>

            <div class="button-group">
                <button class="btn-prev" onclick="prevQuestion()" id="prevBtn" style="display: none;">이전</button>
                <button class="btn-next" onclick="nextQuestion()" id="nextBtn" disabled>다음</button>
            </div>
        </div>

        <!-- 결과 화면 -->
        <div id="resultScreen" class="result-container">
            <h1>🎉 추천 여행지</h1>
            <div class="subtitle">당신에게 완벽한 여행지를 찾았어요!</div>
            <div id="resultContent"></div>
            <button class="restart-btn" onclick="restart()">다시 시작하기</button>
        </div>

        <!-- 최종 선택 화면 -->
        <div id="finalChoice" class="final-choice">
            <h1>✈️ 최종 선택</h1>
            <div class="subtitle">멋진 선택이에요!</div>
            <div id="finalContent"></div>
            <button class="restart-btn" onclick="restart()">다시 시작하기</button>
        </div>
    </div>

    <script>
        // 국가별 좌표 데이터 (위도, 경도)
        const countryCoordinates = {
            '미국': { lat: 37.0902, lng: -95.7129, zoom: 4 },
            '일본': { lat: 36.2048, lng: 138.2529, zoom: 5 },
            '프랑스': { lat: 46.2276, lng: 2.2137, zoom: 6 },
            '태국': { lat: 15.8700, lng: 100.9925, zoom: 6 },
            '뉴질랜드': { lat: -40.9006, lng: 174.8860, zoom: 5 },
            '이탈리아': { lat: 41.8719, lng: 12.5674, zoom: 6 },
            '스페인': { lat: 40.4637, lng: -3.7492, zoom: 6 },
            '그리스': { lat: 39.0742, lng: 21.8243, zoom: 6 },
            '호주': { lat: -25.2744, lng: 133.7751, zoom: 4 },
            '포르투갈': { lat: 39.3999, lng: -8.2245, zoom: 6 },
            '캐나다': { lat: 56.1304, lng: -106.3468, zoom: 4 },
            '영국': { lat: 55.3781, lng: -3.4360, zoom: 6 },
            '독일': { lat: 51.1657, lng: 10.4515, zoom: 6 },
            '스위스': { lat: 46.8182, lng: 8.2275, zoom: 7 },
            '네덜란드': { lat: 52.1326, lng: 5.2913, zoom: 7 },
            '벨기에': { lat: 50.5039, lng: 4.4699, zoom: 7 },
            '오스트리아': { lat: 47.5162, lng: 14.5501, zoom: 7 },
            '체코': { lat: 49.8175, lng: 15.4730, zoom: 7 },
            '폴란드': { lat: 51.9194, lng: 19.1451, zoom: 6 },
            '헝가리': { lat: 47.1625, lng: 19.5033, zoom: 7 },
            '크로아티아': { lat: 45.1, lng: 15.2, zoom: 7 },
            '터키': { lat: 38.9637, lng: 35.2433, zoom: 6 },
            '아이슬란드': { lat: 64.9631, lng: -19.0208, zoom: 6 },
            '노르웨이': { lat: 60.4720, lng: 8.4689, zoom: 5 },
            '스웨덴': { lat: 60.1282, lng: 18.6435, zoom: 5 },
            '핀란드': { lat: 61.9241, lng: 25.7482, zoom: 5 },
            '덴마크': { lat: 56.2639, lng: 9.5018, zoom: 7 },
            '싱가포르': { lat: 1.3521, lng: 103.8198, zoom: 11 },
            '베트남': { lat: 14.0583, lng: 108.2772, zoom: 6 },
            '인도네시아': { lat: -0.7893, lng: 113.9213, zoom: 5 },
            '말레이시아': { lat: 4.2105, lng: 101.9758, zoom: 6 },
            '필리핀': { lat: 12.8797, lng: 121.7740, zoom: 6 },
            '인도': { lat: 20.5937, lng: 78.9629, zoom: 5 },
            '네팔': { lat: 28.3949, lng: 84.1240, zoom: 7 },
            '중국': { lat: 35.8617, lng: 104.1954, zoom: 4 },
            '한국': { lat: 35.9078, lng: 127.7669, zoom: 7 },
            '대만': { lat: 23.6978, lng: 120.9605, zoom: 7 },
            '홍콩': { lat: 22.3193, lng: 114.1694, zoom: 11 },
            '몽골': { lat: 46.8625, lng: 103.8467, zoom: 5 },
            '멕시코': { lat: 23.6345, lng: -102.5528, zoom: 5 },
            '브라질': { lat: -14.2350, lng: -51.9253, zoom: 4 },
            '아르헨티나': { lat: -38.4161, lng: -63.6167, zoom: 4 },
            '칠레': { lat: -35.6751, lng: -71.5430, zoom: 4 },
            '페루': { lat: -9.1900, lng: -75.0152, zoom: 5 },
            '콜롬비아': { lat: 4.5709, lng: -74.2973, zoom: 6 },
            '에콰도르': { lat: -1.8312, lng: -78.1834, zoom: 6 },
            '볼리비아': { lat: -16.2902, lng: -63.5887, zoom: 6 },
            '우루과이': { lat: -32.5228, lng: -55.7658, zoom: 7 },
            '이집트': { lat: 26.8206, lng: 30.8025, zoom: 6 },
            '모로코': { lat: 31.7917, lng: -7.0926, zoom: 6 },
            '남아프리카공화국': { lat: -30.5595, lng: 22.9375, zoom: 5 },
            '케냐': { lat: -0.0236, lng: 37.9062, zoom: 6 },
            '탄자니아': { lat: -6.3690, lng: 34.8888, zoom: 6 },
            '보츠와나': { lat: -22.3285, lng: 24.6849, zoom: 6 },
            '나미비아': { lat: -22.9576, lng: 18.4904, zoom: 6 },
            '마다가스카르': { lat: -18.7669, lng: 46.8691, zoom: 6 },
            '세이셸': { lat: -4.6796, lng: 55.4920, zoom: 10 },
            '모리셔스': { lat: -20.3484, lng: 57.5522, zoom: 10 },
            '몰디브': { lat: 3.2028, lng: 73.2207, zoom: 8 },
            '두바이': { lat: 25.2048, lng: 55.2708, zoom: 10 },
            '요르단': { lat: 30.5852, lng: 36.2384, zoom: 7 },
            '이스라엘': { lat: 31.0461, lng: 34.8516, zoom: 8 },
            '오만': { lat: 21.4735, lng: 55.9754, zoom: 6 },
            '카타르': { lat: 25.3548, lng: 51.1839, zoom: 9 },
            '러시아': { lat: 61.5240, lng: 105.3188, zoom: 3 },
            '우크라이나': { lat: 48.3794, lng: 31.1656, zoom: 6 },
            '조지아': { lat: 42.3154, lng: 43.3569, zoom: 7 },
            '아르메니아': { lat: 40.0691, lng: 45.0382, zoom: 8 },
            '아제르바이잔': { lat: 40.1431, lng: 47.5769, zoom: 7 },
            '카자흐스탄': { lat: 48.0196, lng: 66.9237, zoom: 5 },
            '우즈베키스탄': { lat: 41.3775, lng: 64.5853, zoom: 6 },
            '키르기스스탄': { lat: 41.2044, lng: 74.7661, zoom: 7 },
            '파키스탄': { lat: 30.3753, lng: 69.3451, zoom: 6 },
            '스리랑카': { lat: 7.8731, lng: 80.7718, zoom: 7 },
            '미얀마': { lat: 21.9162, lng: 95.9560, zoom: 6 },
            '라오스': { lat: 19.8563, lng: 102.4955, zoom: 6 },
            '캄보디아': { lat: 12.5657, lng: 104.9910, zoom: 7 },
            '부탄': { lat: 27.5142, lng: 90.4336, zoom: 8 },
            '아일랜드': { lat: 53.4129, lng: -8.2439, zoom: 7 },
            '루마니아': { lat: 45.9432, lng: 24.9668, zoom: 7 },
            '불가리아': { lat: 42.7339, lng: 25.4858, zoom: 7 },
            '세르비아': { lat: 44.0165, lng: 21.0059, zoom: 7 },
            '슬로베니아': { lat: 46.1512, lng: 14.9955, zoom: 8 },
            '몬테네그로': { lat: 42.7087, lng: 19.3744, zoom: 8 },
            '알바니아': { lat: 41.1533, lng: 20.1683, zoom: 8 },
            '몰타': { lat: 35.9375, lng: 14.3754, zoom: 10 },
            '키프로스': { lat: 35.1264, lng: 33.4299, zoom: 9 },
            '에스토니아': { lat: 58.5953, lng: 25.0136, zoom: 7 },
            '라트비아': { lat: 56.8796, lng: 24.6032, zoom: 7 },
            '리투아니아': { lat: 55.1694, lng: 23.8813, zoom: 7 },
            '뉴칼레도니아': { lat: -20.9043, lng: 165.6180, zoom: 8 },
            '피지': { lat: -17.7134, lng: 178.0650, zoom: 8 },
            '타히티': { lat: -17.6509, lng: -149.4260, zoom: 9 },
            '르완다': { lat: -1.9403, lng: 29.8739, zoom: 8 },
            '에티오피아': { lat: 9.1450, lng: 40.4897, zoom: 6 },
            '방글라데시': { lat: 23.6850, lng: 90.3563, zoom: 7 }
        };

        // 국가 데이터베이스 (100개 국가)
        const destinations = [
            { name: '미국', flag: '🇺🇸', tags: ['도시', '자연', '다양', '문화', '쇼핑'], climate: 'temperate', budget: 'high', vibe: 'both', mbti: ['ENTJ', 'ESTJ', 'ENTP', 'ESTP'], desc: '광활한 대륙에 다양한 문화와 자연이 공존하는 나라. 뉴욕, LA 같은 대도시부터 그랜드캐니언, 옐로스톤 같은 자연까지 모든 것을 경험할 수 있어요.' },
            { name: '일본', flag: '🇯🇵', tags: ['문화', '음식', '도시', '안전', '쇼핑'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ISTJ', 'ISFJ', 'ESTJ', 'ESFJ'], desc: '전통과 현대가 공존하는 매력적인 나라. 깨끗하고 안전하며 대중교통이 발달해 있어요.' },
            { name: '프랑스', flag: '🇫🇷', tags: ['문화', '예술', '음식', '로맨틱', '역사'], climate: 'temperate', budget: 'high', vibe: 'city', mbti: ['INFP', 'ENFP', 'INFJ', 'ENFJ'], desc: '예술과 낭만의 나라. 세계적인 미술관과 요리, 아름다운 시골 풍경을 즐길 수 있어요.' },
            { name: '태국', flag: '🇹🇭', tags: ['해변', '음식', '저렴', '사원', '나이트라이프'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['ESFP', 'ESTP', 'ENFP', 'ENTP'], desc: '활기찬 거리, 맛있는 음식, 아름다운 해변이 있는 동남아시아의 보석.' },
            { name: '뉴질랜드', flag: '🇳🇿', tags: ['자연', '액티비티', '모험', '청정'], climate: 'temperate', budget: 'high', vibe: 'nature', mbti: ['ISTP', 'ESTP', 'ISFP', 'ESFP'], desc: '숨막히는 자연 경관과 다양한 모험 액티비티를 즐길 수 있어요.' },
            { name: '이탈리아', flag: '🇮🇹', tags: ['문화', '음식', '역사', '예술', '로맨틱'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ENFP', 'ESFP', 'INFP', 'ISFP'], desc: '역사, 예술, 음식의 완벽한 조화를 경험할 수 있어요.' },
            { name: '스페인', flag: '🇪🇸', tags: ['문화', '음식', '나이트라이프', '해변', '축제'], climate: 'warm', budget: 'medium', vibe: 'city', mbti: ['ESFP', 'ESTP', 'ENFP', 'ENTP'], desc: '열정적인 문화와 맛있는 음식, 아름다운 해변이 있어요.' },
            { name: '그리스', flag: '🇬🇷', tags: ['역사', '해변', '문화', '로맨틱', '섬'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ENFP', 'INFP', 'ENFJ', 'INFJ'], desc: '고대 문명의 흔적과 에메랄드빛 바다를 만날 수 있어요.' },
            { name: '호주', flag: '🇦🇺', tags: ['자연', '해변', '액티비티', '동물', '도시'], climate: 'warm', budget: 'high', vibe: 'both', mbti: ['ESTP', 'ESFP', 'ISTP', 'ISFP'], desc: '대자연과 현대 도시가 공존하는 나라예요.' },
            { name: '포르투갈', flag: '🇵🇹', tags: ['문화', '음식', '해변', '저렴', '역사'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ISFP', 'INFP', 'ESFP', 'ENFP'], desc: '유럽에서 가성비 좋은 여행지로 아름다운 해안 도시가 매력적이에요.' },
            { name: '캐나다', flag: '🇨🇦', tags: ['자연', '액티비티', '청정', '다문화', '안전'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['ISTJ', 'ISFJ', 'INTJ', 'INFJ'], desc: '광활한 자연과 깨끗한 도시를 경험할 수 있어요.' },
            { name: '영국', flag: '🇬🇧', tags: ['문화', '역사', '도시', '박물관', '펍'], climate: 'temperate', budget: 'high', vibe: 'city', mbti: ['INTJ', 'INTP', 'ENTJ', 'ENTP'], desc: '역사와 현대가 공존하는 나라예요.' },
            { name: '독일', flag: '🇩🇪', tags: ['문화', '역사', '맥주', '도시', '성'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ISTJ', 'INTJ', 'ESTJ', 'ENTJ'], desc: '효율성과 전통이 조화를 이루는 나라예요.' },
            { name: '스위스', flag: '🇨🇭', tags: ['자연', '산', '고급', '청정', '스키'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['ISTJ', 'INTJ', 'ISFJ', 'INFJ'], desc: '그림 같은 알프스 풍경을 즐길 수 있어요.' },
            { name: '네덜란드', flag: '🇳🇱', tags: ['문화', '도시', '자전거', '예술', '자유'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ENTP', 'ENFP', 'INTP', 'INFP'], desc: '자전거와 튤립의 나라로 자유로운 분위기가 특징이에요.' },
            { name: '벨기에', flag: '🇧🇪', tags: ['음식', '문화', '초콜릿', '맥주', '건축'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ISFJ', 'ESFJ', 'INFP', 'ENFP'], desc: '초콜릿과 맥주의 천국이에요.' },
            { name: '오스트리아', flag: '🇦🇹', tags: ['문화', '음악', '역사', '산', '고급'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['INFJ', 'INFP', 'INTJ', 'INTP'], desc: '클래식 음악과 우아한 건축물이 매력적이에요.' },
            { name: '체코', flag: '🇨🇿', tags: ['문화', '역사', '저렴', '맥주', '건축'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '동화 같은 프라하의 거리가 아름다워요.' },
            { name: '폴란드', flag: '🇵🇱', tags: ['역사', '문화', '저렴', '음식'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['ISTJ', 'ISFJ', 'INTJ', 'INFJ'], desc: '복잡한 역사를 간직한 나라예요.' },
            { name: '헝가리', flag: '🇭🇺', tags: ['문화', '온천', '저렴', '건축', '음식'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['INFP', 'ISFP', 'ENFP', 'ESFP'], desc: '부다페스트의 야경과 온천이 유명해요.' },
            { name: '크로아티아', flag: '🇭🇷', tags: ['해변', '역사', '문화', '저렴', '섬'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ISFP', 'INFP', 'ESFP', 'ENFP'], desc: '아드리아해의 진주로 불리는 아름다운 나라예요.' },
            { name: '터키', flag: '🇹🇷', tags: ['문화', '역사', '음식', '저렴', '독특'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ENTP', 'ENFP', 'INTP', 'INFP'], desc: '동서양이 만나는 독특한 문화를 경험할 수 있어요.' },
            { name: '아이슬란드', flag: '🇮🇸', tags: ['자연', '오로라', '온천', '독특'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['INTJ', 'INTP', 'INFJ', 'INFP'], desc: '신비로운 자연 현상을 볼 수 있어요.' },
            { name: '노르웨이', flag: '🇳🇴', tags: ['자연', '오로라', '피요르드', '액티비티'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['INTJ', 'INTP', 'ISTJ', 'ISTP'], desc: '장엄한 피요르드와 오로라가 유명해요.' },
            { name: '스웨덴', flag: '🇸🇪', tags: ['자연', '도시', '디자인', '청정', '북유럽'], climate: 'cold', budget: 'high', vibe: 'both', mbti: ['INTJ', 'INTP', 'INFJ', 'INFP'], desc: '북유럽의 혁신과 자연을 경험할 수 있어요.' },
            { name: '핀란드', flag: '🇫🇮', tags: ['자연', '오로라', '사우나', '청정', '독특'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['INTJ', 'INTP', 'ISTJ', 'ISTP'], desc: '천 개의 호수와 오로라가 아름다워요.' },
            { name: '덴마크', flag: '🇩🇰', tags: ['문화', '도시', '디자인', '자전거', '행복'], climate: 'temperate', budget: 'high', vibe: 'city', mbti: ['INFJ', 'INFP', 'INTJ', 'INTP'], desc: '행복한 나라로 유명해요.' },
            { name: '싱가포르', flag: '🇸🇬', tags: ['도시', '음식', '쇼핑', '안전', '현대'], climate: 'tropical', budget: 'high', vibe: 'city', mbti: ['ISTJ', 'ESTJ', 'INTJ', 'ENTJ'], desc: '미래형 도시 국가예요.' },
            { name: '베트남', flag: '🇻🇳', tags: ['음식', '저렴', '문화', '해변', '역사'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['ISFP', 'ESFP', 'INFP', 'ENFP'], desc: '맛있는 음식과 저렴한 물가가 매력적이에요.' },
            { name: '인도네시아', flag: '🇮🇩', tags: ['해변', '문화', '저렴', '사원', '서핑'], climate: 'tropical', budget: 'low', vibe: 'nature', mbti: ['ISFP', 'ESFP', 'INFP', 'ENFP'], desc: '발리를 포함한 아름다운 섬들이 많아요.' },
            { name: '말레이시아', flag: '🇲🇾', tags: ['음식', '저렴', '문화', '해변', '다문화'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['ESFP', 'ISFP', 'ENFP', 'INFP'], desc: '다양한 문화가 공존하는 나라예요.' },
            { name: '필리핀', flag: '🇵🇭', tags: ['해변', '저렴', '다이빙', '섬', '친절'], climate: 'tropical', budget: 'low', vibe: 'nature', mbti: ['ESFP', 'ISFP', 'ENFP', 'INFP'], desc: '7천 개 이상의 아름다운 섬이 있어요.' },
            { name: '인도', flag: '🇮🇳', tags: ['문화', '역사', '음식', '저렴', '영적'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['INFP', 'ENFP', 'INTP', 'ENTP'], desc: '다채로운 색과 문화를 경험할 수 있어요.' },
            { name: '네팔', flag: '🇳🇵', tags: ['산', '모험', '문화', '트레킹', '영적'], climate: 'temperate', budget: 'low', vibe: 'nature', mbti: ['INFP', 'INTP', 'ISFP', 'ISTP'], desc: '히말라야 트레킹의 성지예요.' },
            { name: '중국', flag: '🇨🇳', tags: ['역사', '문화', '음식', '도시', '다양'], climate: 'temperate', budget: 'medium', vibe: 'both', mbti: ['ENTJ', 'INTJ', 'ESTJ', 'ISTJ'], desc: '고대와 현대가 공존하는 거대한 나라예요.' },
            { name: '한국', flag: '🇰🇷', tags: ['문화', '음식', '도시', '쇼핑', '현대'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ISTJ', 'ESTJ', 'ISFJ', 'ESFJ'], desc: 'K-문화의 발상지로 활기찬 도시예요.' },
            { name: '대만', flag: '🇹🇼', tags: ['음식', '문화', '저렴', '자연', '친절'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['ISFP', 'ESFP', 'INFP', 'ENFP'], desc: '맛있는 야시장 음식이 유명해요.' },
            { name: '홍콩', flag: '🇭🇰', tags: ['도시', '음식', '쇼핑', '현대', '문화'], climate: 'tropical', budget: 'high', vibe: 'city', mbti: ['ESTJ', 'ENTJ', 'ESTP', 'ENTP'], desc: '동양과 서양이 만나는 역동적인 도시예요.' },
            { name: '몽골', flag: '🇲🇳', tags: ['자연', '모험', '독특', '유목', '문화'], climate: 'cold', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '광활한 초원과 유목 문화를 경험할 수 있어요.' },
            { name: '멕시코', flag: '🇲🇽', tags: ['문화', '음식', '해변', '역사', '저렴'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ESFP', 'ESTP', 'ENFP', 'ENTP'], desc: '활기찬 문화와 맛있는 음식이 있어요.' },
            { name: '브라질', flag: '🇧🇷', tags: ['문화', '해변', '축제', '자연', '활기'], climate: 'tropical', budget: 'medium', vibe: 'both', mbti: ['ESFP', 'ESTP', 'ENFP', 'ENTP'], desc: '삼바와 축구의 나라예요.' },
            { name: '아르헨티나', flag: '🇦🇷', tags: ['문화', '음식', '자연', '탱고', '와인'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ESFP', 'ENFP', 'ESTP', 'ENTP'], desc: '열정적인 탱고와 맛있는 스테이크가 유명해요.' },
            { name: '칠레', flag: '🇨🇱', tags: ['자연', '와인', '모험', '산', '해변'], climate: 'temperate', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '길고 다양한 지형의 나라예요.' },
            { name: '페루', flag: '🇵🇪', tags: ['역사', '모험', '문화', '산', '독특'], climate: 'temperate', budget: 'low', vibe: 'nature', mbti: ['ENTP', 'ENFP', 'INTP', 'INFP'], desc: '마추픽추의 신비를 경험할 수 있어요.' },
            { name: '콜롬비아', flag: '🇨🇴', tags: ['문화', '음식', '저렴', '커피', '활기'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['ESFP', 'ENFP', 'ESTP', 'ENTP'], desc: '커피와 살사의 나라예요.' },
            { name: '에콰도르', flag: '🇪🇨', tags: ['자연', '모험', '독특', '다양', '저렴'], climate: 'temperate', budget: 'low', vibe: 'nature', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '갈라파고스 제도로 유명해요.' },
            { name: '볼리비아', flag: '🇧🇴', tags: ['자연', '모험', '저렴', '독특', '산'], climate: 'temperate', budget: 'low', vibe: 'nature', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '우유니 소금사막이 장관이에요.' },
            { name: '우루과이', flag: '🇺🇾', tags: ['해변', '와인', '문화', '저렴', '휴식'], climate: 'temperate', budget: 'medium', vibe: 'both', mbti: ['ISFP', 'INFP', 'ESFP', 'ENFP'], desc: '남미의 숨겨진 보석이에요.' },
            { name: '아이슬란드', flag: '🇮🇸', tags: ['자연', '오로라', '온천', '독특'], climate: 'cold', budget: 'high', vibe: 'nature', mbti: ['INTJ', 'INTP', 'INFJ', 'INFP'], desc: '신비로운 자연 현상을 볼 수 있어요.' },
            { name: '이집트', flag: '🇪🇬', tags: ['역사', '문화', '모험', '사막', '독특'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ENTP', 'INTP', 'ENFP', 'INFP'], desc: '고대 문명의 신비를 경험할 수 있어요.' },
            { name: '모로코', flag: '🇲🇦', tags: ['문화', '모험', '독특', '시장', '사막'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ENTP', 'ENFP', 'ESTP', 'ESFP'], desc: '이국적인 분위기가 매력적이에요.' },
            { name: '남아프리카공화국', flag: '🇿🇦', tags: ['자연', '야생동물', '모험', '와인'], climate: 'warm', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'ESTP', 'ISFP', 'ESFP'], desc: '사파리와 와인의 나라예요.' },
            { name: '케냐', flag: '🇰🇪', tags: ['자연', '야생동물', '사파리', '모험'], climate: 'warm', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'ESTP', 'ISFP', 'ESFP'], desc: '아프리카 사파리의 정수예요.' },
            { name: '탄자니아', flag: '🇹🇿', tags: ['자연', '야생동물', '산', '사파리', '모험'], climate: 'warm', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'ESTP', 'INTP', 'ENTP'], desc: '킬리만자로와 세렝게티가 유명해요.' },
            { name: '보츠와나', flag: '🇧🇼', tags: ['자연', '야생동물', '사파리', '고급', '모험'], climate: 'warm', budget: 'high', vibe: 'nature', mbti: ['ISTP', 'ESTP', 'INTJ', 'ENTJ'], desc: '프리미엄 사파리의 성지예요.' },
            { name: '나미비아', flag: '🇳🇦', tags: ['자연', '사막', '야생동물', '모험', '독특'], climate: 'warm', budget: 'medium', vibe: 'nature', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '아프리카의 광활한 풍경이 장관이에요.' },
            { name: '마다가스카르', flag: '🇲🇬', tags: ['자연', '야생동물', '독특', '해변', '모험'], climate: 'tropical', budget: 'medium', vibe: 'nature', mbti: ['INTP', 'INFP', 'ISTP', 'ISFP'], desc: '독특한 동식물을 만날 수 있어요.' },
            { name: '세이셸', flag: '🇸🇨', tags: ['해변', '고급', '허니문', '자연', '휴식'], climate: 'tropical', budget: 'high', vibe: 'nature', mbti: ['INFP', 'ISFP', 'ENFP', 'ESFP'], desc: '숨겨진 열대 천국이에요.' },
            { name: '모리셔스', flag: '🇲🇺', tags: ['해변', '고급', '허니문', '다이빙', '휴식'], climate: 'tropical', budget: 'high', vibe: 'nature', mbti: ['ISFJ', 'INFJ', 'ESFJ', 'ENFJ'], desc: '인도양의 낙원이에요.' },
            { name: '몰디브', flag: '🇲🇻', tags: ['해변', '고급', '허니문', '다이빙', '휴식'], climate: 'tropical', budget: 'high', vibe: 'nature', mbti: ['ISFJ', 'INFJ', 'ISFP', 'INFP'], desc: '천국 같은 리조트 섬이에요.' },
            { name: '두바이', flag: '🇦🇪', tags: ['도시', '고급', '쇼핑', '현대', '독특'], climate: 'warm', budget: 'high', vibe: 'city', mbti: ['ENTJ', 'ESTJ', 'ESTP', 'ESFP'], desc: '미래형 메가시티예요.' },
            { name: '요르단', flag: '🇯🇴', tags: ['역사', '모험', '문화', '사막', '독특'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ENTP', 'INTP', 'ENFP', 'INFP'], desc: '페트라와 사해의 나라예요.' },
            { name: '이스라엘', flag: '🇮🇱', tags: ['역사', '문화', '종교', '해변', '도시'], climate: 'warm', budget: 'high', vibe: 'both', mbti: ['INTJ', 'INTP', 'ENTJ', 'ENTP'], desc: '세 종교의 성지예요.' },
            { name: '오만', flag: '🇴🇲', tags: ['문화', '사막', '해변', '독특'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '아라비아의 숨은 보석이에요.' },
            { name: '카타르', flag: '🇶🇦', tags: ['도시', '고급', '문화', '현대', '쇼핑'], climate: 'warm', budget: 'high', vibe: 'city', mbti: ['ENTJ', 'ESTJ', 'INTJ', 'ISTJ'], desc: '미래형 사막 도시예요.' },
            { name: '러시아', flag: '🇷🇺', tags: ['역사', '문화', '건축', '독특', '거대'], climate: 'cold', budget: 'medium', vibe: 'city', mbti: ['INTJ', 'INTP', 'ENTJ', 'ENTP'], desc: '광활한 대륙을 경험할 수 있어요.' },
            { name: '우크라이나', flag: '🇺🇦', tags: ['역사', '문화', '저렴', '음식'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['INFP', 'ISFP', 'ENFP', 'ESFP'], desc: '숨겨진 동유럽의 보석이에요.' },
            { name: '조지아', flag: '🇬🇪', tags: ['문화', '음식', '와인', '산', '저렴'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ENFP', 'ESFP', 'INFP', 'ISFP'], desc: '와인의 발상지예요.' },
            { name: '아르메니아', flag: '🇦🇲', tags: ['역사', '문화', '산', '저렴', '독특'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '세계 최초의 기독교 국가예요.' },
            { name: '아제르바이잔', flag: '🇦🇿', tags: ['문화', '도시', '독특', '역사'], climate: 'temperate', budget: 'medium', vibe: 'city', mbti: ['ENTP', 'ENTJ', 'INTP', 'INTJ'], desc: '불의 땅으로 독특한 문화가 있어요.' },
            { name: '카자흐스탄', flag: '🇰🇿', tags: ['자연', '문화', '모험', '독특', '산'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '중앙아시아의 거대한 나라예요.' },
            { name: '우즈베키스탄', flag: '🇺🇿', tags: ['역사', '문화', '건축', '저렴', '독특'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '실크로드의 진주예요.' },
            { name: '키르기스스탄', flag: '🇰🇬', tags: ['자연', '산', '모험', '저렴', '유목'], climate: 'cold', budget: 'low', vibe: 'nature', mbti: ['ISTP', 'ISFP', 'ESTP', 'ESFP'], desc: '중앙아시아의 스위스예요.' },
            { name: '파키스탄', flag: '🇵🇰', tags: ['산', '모험', '문화', '저렴', '독특'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ISTP', 'INTP', 'ESTP', 'ENTP'], desc: '카라코람의 장엄함을 볼 수 있어요.' },
            { name: '스리랑카', flag: '🇱🇰', tags: ['문화', '해변', '차', '야생동물', '저렴'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['INFP', 'ISFP', 'ENFP', 'ESFP'], desc: '인도양의 진주예요.' },
            { name: '미얀마', flag: '🇲🇲', tags: ['문화', '사원', '저렴', '독특'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['INFP', 'ISFP', 'INTP', 'ISTP'], desc: '황금 불탑의 나라예요.' },
            { name: '라오스', flag: '🇱🇦', tags: ['문화', '저렴', '자연', '사원', '휴식'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['INFP', 'ISFP', 'INTP', 'ISTP'], desc: '동남아시아의 숨은 보석이에요.' },
            { name: '캄보디아', flag: '🇰🇭', tags: ['역사', '문화', '저렴', '사원'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['INFP', 'ENFP', 'INTP', 'ENTP'], desc: '앙코르 와트의 나라예요.' },
            { name: '부탄', flag: '🇧🇹', tags: ['문화', '산', '영적', '독특', '고급'], climate: 'temperate', budget: 'high', vibe: 'nature', mbti: ['INFJ', 'INFP', 'INTJ', 'INTP'], desc: '행복의 나라예요.' },
            { name: '아일랜드', flag: '🇮🇪', tags: ['자연', '문화', '펍', '역사', '친절'], climate: 'temperate', budget: 'medium', vibe: 'nature', mbti: ['ENFP', 'INFP', 'ESFP', 'ISFP'], desc: '에메랄드빛 섬이에요.' },
            { name: '루마니아', flag: '🇷🇴', tags: ['역사', '문화', '저렴', '독특', '성'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '드라큘라의 나라예요.' },
            { name: '불가리아', flag: '🇧🇬', tags: ['해변', '산', '저렴', '역사', '스키'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['ISTP', 'ISFP', 'ESTP', 'ESFP'], desc: '동유럽의 가성비 천국이에요.' },
            { name: '세르비아', flag: '🇷🇸', tags: ['문화', '나이트라이프', '저렴', '음식', '역사'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['ESFP', 'ESTP', 'ENFP', 'ENTP'], desc: '발칸 반도의 심장이에요.' },
            { name: '슬로베니아', flag: '🇸🇮', tags: ['자연', '호수', '저렴', '청정', '액티비티'], climate: 'temperate', budget: 'low', vibe: 'nature', mbti: ['ISFP', 'INFP', 'ISTP', 'INTP'], desc: '알프스와 지중해가 만나는 곳이에요.' },
            { name: '몬테네그로', flag: '🇲🇪', tags: ['해변', '자연', '저렴', '액티비티'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ISFP', 'ESFP', 'ISTP', 'ESTP'], desc: '아드리아해의 숨은 보석이에요.' },
            { name: '알바니아', flag: '🇦🇱', tags: ['해변', '저렴', '역사', '독특', '자연'], climate: 'warm', budget: 'low', vibe: 'both', mbti: ['ISTP', 'ESTP', 'ISFP', 'ESFP'], desc: '유럽의 마지막 비밀이에요.' },
            { name: '몰타', flag: '🇲🇹', tags: ['역사', '해변', '문화', '섬'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ENFP', 'ESFP', 'INFP', 'ISFP'], desc: '지중해의 작은 보석이에요.' },
            { name: '키프로스', flag: '🇨🇾', tags: ['해변', '역사', '문화', '와인'], climate: 'warm', budget: 'medium', vibe: 'both', mbti: ['ESFP', 'ENFP', 'ISFP', 'INFP'], desc: '아프로디테의 섬이에요.' },
            { name: '에스토니아', flag: '🇪🇪', tags: ['문화', '도시', '역사', '저렴', '디지털'], climate: 'cold', budget: 'low', vibe: 'city', mbti: ['INTP', 'INTJ', 'ENTP', 'ENTJ'], desc: '디지털 강국의 중세 도시예요.' },
            { name: '라트비아', flag: '🇱🇻', tags: ['문화', '건축', '저렴', '역사', '자연'], climate: 'cold', budget: 'low', vibe: 'both', mbti: ['INFP', 'ISFP', 'INTP', 'ISTP'], desc: '발트해의 아르누보예요.' },
            { name: '리투아니아', flag: '🇱🇹', tags: ['역사', '문화', '저렴', '건축'], climate: 'temperate', budget: 'low', vibe: 'city', mbti: ['ISTJ', 'ISFJ', 'INTJ', 'INFJ'], desc: '발트 3국의 보석이에요.' },
            { name: '뉴칼레도니아', flag: '🇳🇨', tags: ['해변', '고급', '다이빙', '자연', '프랑스령'], climate: 'tropical', budget: 'high', vibe: 'nature', mbti: ['ISFP', 'INFP', 'ESFP', 'ENFP'], desc: '남태평양의 천국이에요.' },
            { name: '피지', flag: '🇫🇯', tags: ['해변', '다이빙', '휴식', '친절', '섬'], climate: 'tropical', budget: 'medium', vibe: 'nature', mbti: ['ESFP', 'ENFP', 'ISFP', 'INFP'], desc: '남태평양의 낙원이에요.' },
            { name: '타히티', flag: '🇵🇫', tags: ['해변', '고급', '허니문', '다이빙', '휴식'], climate: 'tropical', budget: 'high', vibe: 'nature', mbti: ['ISFJ', 'INFJ', 'ISFP', 'INFP'], desc: '프랑스령 폴리네시아의 보석이에요.' },
            { name: '르완다', flag: '🇷🇼', tags: ['자연', '야생동물', '고릴라', '안전'], climate: 'temperate', budget: 'high', vibe: 'nature', mbti: ['INFJ', 'INTJ', 'ISFJ', 'ISTJ'], desc: '아프리카의 싱가포르예요.' },
            { name: '에티오피아', flag: '🇪🇹', tags: ['역사', '문화', '독특', '커피', '모험'], climate: 'temperate', budget: 'low', vibe: 'both', mbti: ['INTP', 'INFP', 'ENTP', 'ENFP'], desc: '인류의 발상지예요.' },
            { name: '방글라데시', flag: '🇧🇩', tags: ['문화', '저렴', '독특', '역사'], climate: 'tropical', budget: 'low', vibe: 'both', mbti: ['INFP', 'ISFP', 'ENFP', 'ESFP'], desc: '천 개의 강의 나라예요.' }
        ];

        // 질문 데이터
        const questions = [
            {
                id: 1,
                question: "당신의 MBTI는?",
                options: [
                    { text: "ISTJ - 세상의 소금형", value: "ISTJ" },
                    { text: "ISFJ - 용감한 수호자형", value: "ISFJ" },
                    { text: "INFJ - 선의의 옹호자형", value: "INFJ" },
                    { text: "INTJ - 용의주도한 전략가형", value: "INTJ" },
                    { text: "ISTP - 만능 재주꾼형", value: "ISTP" },
                    { text: "ISFP - 호기심 많은 예술가형", value: "ISFP" },
                    { text: "INFP - 열정적인 중재자형", value: "INFP" },
                    { text: "INTP - 논리적인 사색가형", value: "INTP" },
                    { text: "ESTP - 모험을 즐기는 사업가형", value: "ESTP" },
                    { text: "ESFP - 자유로운 영혼의 연예인형", value: "ESFP" },
                    { text: "ENFP - 재기발랄한 활동가형", value: "ENFP" },
                    { text: "ENTP - 뜨거운 논쟁을 즐기는 변론가형", value: "ENTP" },
                    { text: "ESTJ - 엄격한 관리자형", value: "ESTJ" },
                    { text: "ESFJ - 사교적인 외교관형", value: "ESFJ" },
                    { text: "ENFJ - 정의로운 사회운동가형", value: "ENFJ" },
                    { text: "ENTJ - 대담한 통솔자형", value: "ENTJ" }
                ]
            },
            {
                id: 2,
                question: "선호하는 여행 스타일은?",
                options: [
                    { text: "🏔️ 모험과 액티비티 (등산, 다이빙, 익스트림 스포츠)", value: "adventure" },
                    { text: "🏖️ 여유로운 휴식 (리조트, 스파, 해변)", value: "relaxation" },
                    { text: "🏛️ 문화와 역사 탐방 (박물관, 유적지, 예술)", value: "culture" },
                    { text: "🍜 미식 여행 (현지 음식, 맛집 투어)", value: "food" },
                    { text: "🎉 활기찬 나이트라이프 (클럽, 바, 축제)", value: "nightlife" }
                ]
            },
            {
                id: 3,
                question: "선호하는 기후는?",
                options: [
                    { text: "☀️ 따뜻하고 화창한 날씨", value: "warm" },
                    { text: "🌡️ 온화한 봄/가을 날씨", value: "temperate" },
                    { text: "🌴 덥고 습한 열대 기후", value: "tropical" },
                    { text: "❄️ 서늘하고 시원한 날씨", value: "cold" },
                    { text: "🤷 날씨는 중요하지 않아요", value: "any" }
                ]
            },
            {
                id: 4,
                question: "여행 시 가장 관심있는 활동은?",
                options: [
                    { text: "🏛️ 역사 유적지와 박물관 관람", value: "history" },
                    { text: "🌿 자연 경관과 야생동물 관찰", value: "nature" },
                    { text: "🛍️ 쇼핑과 현지 시장 구경", value: "shopping" },
                    { text: "📸 인스타그램에 올릴 멋진 사진 촬영", value: "photo" },
                    { text: "🤝 현지인들과 교류하고 문화 체험", value: "local" }
                ]
            },
            {
                id: 5,
                question: "누구와 함께 여행하나요?",
                options: [
                    { text: "🧍 혼자 자유롭게", value: "solo" },
                    { text: "💑 연인과 로맨틱하게", value: "couple" },
                    { text: "👨‍👩‍👧‍👦 가족과 함께", value: "family" },
                    { text: "👥 친구들과 활기차게", value: "friends" },
                    { text: "👔 비즈니스/워케이션", value: "business" }
                ]
            },
            {
                id: 6,
                question: "여행 예산은 어느 정도인가요?",
                options: [
                    { text: "💰 저렴하게 (백패킹, 게스트하우스)", value: "low" },
                    { text: "💵 적당하게 (일반 호텔, 대중교통)", value: "medium" },
                    { text: "💎 여유롭게 (고급 호텔, 편안한 여행)", value: "high" },
                    { text: "✨ 럭셔리하게 (최고급 리조트, 프리미엄 경험)", value: "luxury" }
                ]
            },
            {
                id: 7,
                question: "선호하는 여행지 분위기는?",
                options: [
                    { text: "🏙️ 활기찬 대도시", value: "city" },
                    { text: "🏞️ 한적한 자연/시골", value: "nature" },
                    { text: "🏖️ 휴양지/리조트", value: "resort" },
                    { text: "🏘️ 작은 마을/로컬 동네", value: "town" },
                    { text: "⚖️ 도시와 자연 모두", value: "both" }
                ]
            },
            {
                id: 8,
                question: "새로운 경험에 대한 태도는?",
                options: [
                    { text: "🚀 완전히 새롭고 독특한 것을 찾아요", value: "extreme" },
                    { text: "🌟 새로운 시도를 좋아해요", value: "adventurous" },
                    { text: "⚖️ 적당한 수준의 새로움이 좋아요", value: "moderate" },
                    { text: "🛡️ 익숙하고 안전한 것을 선호해요", value: "safe" }
                ]
            },
            {
                id: 9,
                question: "선호하는 여행 기간은?",
                options: [
                    { text: "⚡ 3일 이내 (주말 여행)", value: "short" },
                    { text: "📅 4-7일 (일주일)", value: "week" },
                    { text: "🗓️ 1-2주", value: "twoweeks" },
                    { text: "🌍 2주 이상 (장기 여행)", value: "long" }
                ]
            },
            {
                id: 10,
                question: "여행에서 가장 중요한 것은?",
                options: [
                    { text: "✅ 안전하고 편안한 환경", value: "safety" },
                    { text: "💰 가성비 좋은 경험", value: "value" },
                    { text: "📷 인생샷과 추억 만들기", value: "memory" },
                    { text: "🎭 독특하고 특별한 경험", value: "unique" },
                    { text: "😌 진정한 휴식과 재충전", value: "rest" }
                ]
            }
        ];

        let currentQuestion = 0;
        let answers = {};

        function startQuiz() {
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('quizScreen').style.display = 'block';
            renderQuestions();
            showQuestion(0);
        }

        function renderQuestions() {
            const container = document.getElementById('questionsContainer');
            container.innerHTML = '';
            
            questions.forEach((q, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question-container';
                questionDiv.id = `question-${index}`;
                
                let optionsHTML = '';
                q.options.forEach((option, optIndex) => {
                    optionsHTML += `
                        <div class="option" onclick="selectOption(${index}, '${option.value}', this)">
                            ${option.text}
                        </div>
                    `;
                });
                
                questionDiv.innerHTML = `
                    <div class="question">${q.question}</div>
                    <div class="options">
                        ${optionsHTML}
                    </div>
                `;
                
                container.appendChild(questionDiv);
            });
        }

        function showQuestion(index) {
            document.querySelectorAll('.question-container').forEach(q => {
                q.classList.remove('active');
            });
            
            document.getElementById(`question-${index}`).classList.add('active');
            
            const progress = ((index + 1) / questions.length) * 100;
            document.getElementById('progressFill').style.width = progress + '%';
            
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');
            
            prevBtn.style.display = index > 0 ? 'block' : 'none';
            
            if (answers[index]) {
                nextBtn.disabled = false;
                nextBtn.textContent = index === questions.length - 1 ? '결과 보기' : '다음';
            } else {
                nextBtn.disabled = true;
                nextBtn.textContent = '다음';
            }
        }

        function selectOption(questionIndex, value, element) {
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => opt.classList.remove('selected'));
            
            element.classList.add('selected');
            answers[questionIndex] = value;
            
            document.getElementById('nextBtn').disabled = false;
            document.getElementById('nextBtn').textContent = 
                questionIndex === questions.length - 1 ? '결과 보기' : '다음';
        }

        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                showQuestion(currentQuestion);
            } else {
                showResults();
            }
        }

        function prevQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion(currentQuestion);
            }
        }

        function calculateResults() {
            const userMBTI = answers[0];
            const scores = {};
            
            destinations.forEach(dest => {
                let score = 0;
                
                if (dest.mbti && dest.mbti.includes(userMBTI)) {
                    score += 30;
                }
                
                if (answers[1]) {
                    const style = answers[1];
                    if (style === 'adventure' && dest.tags.includes('액티비티')) score += 15;
                    if (style === 'relaxation' && dest.tags.includes('해변')) score += 15;
                    if (style === 'culture' && dest.tags.includes('문화')) score += 15;
                    if (style === 'food' && dest.tags.includes('음식')) score += 15;
                    if (style === 'nightlife' && dest.tags.includes('나이트라이프')) score += 15;
                }
                
                if (answers[2] && answers[2] !== 'any') {
                    if (dest.climate === answers[2]) score += 20;
                }
                
                if (answers[3]) {
                    const activity = answers[3];
                    if (activity === 'history' && dest.tags.includes('역사')) score += 10;
                    if (activity === 'nature' && dest.tags.includes('자연')) score += 10;
                    if (activity === 'shopping' && dest.tags.includes('쇼핑')) score += 10;
                    if (activity === 'photo' && (dest.tags.includes('해변') || dest.tags.includes('자연'))) score += 10;
                    if (activity === 'local' && dest.tags.includes('문화')) score += 10;
                }
                
                if (answers[5]) {
                    if (dest.budget === answers[5]) score += 15;
                }
                
                if (answers[6]) {
                    if (dest.vibe === answers[6] || dest.vibe === 'both') score += 10;
                }
                
                if (answers[7]) {
                    if (answers[7] === 'extreme' && dest.tags.includes('독특')) score += 10;
                    if (answers[7] === 'safe' && dest.tags.includes('안전')) score += 10;
                }
                
                if (answers[9]) {
                    if (answers[9] === 'safety' && dest.tags.includes('안전')) score += 10;
                    if (answers[9] === 'value' && dest.tags.includes('저렴')) score += 10;
                    if (answers[9] === 'unique' && dest.tags.includes('독특')) score += 10;
                }
                
                scores[dest.name] = score;
            });
            
            const sortedDestinations = Object.entries(scores)
                .sort((a, b) => b[1] - a[1])
                .slice(0, 3)
                .map(([name, score]) => ({
                    ...destinations.find(d => d.name === name),
                    score: score
                }));
            
            return sortedDestinations;
        }

        function showResults() {
            document.getElementById('quizScreen').style.display = 'none';
            document.getElementById('resultScreen').style.display = 'block';
            
            const results = calculateResults();
            const resultContent = document.getElementById('resultContent');
            
            let html = '';
            
            results.forEach((dest, index) => {
                const matchPercentage = Math.round((dest.score / 100) * 100);
                html += `
                    <div class="destination" id="dest-${index}">
                        <h3>
                            <div class="title-section">
                                <span class="flag">${dest.flag}</span>
                                <span class="country-name">${index + 1}. ${dest.name}</span>
                            </div>
                            <span class="match-score">매칭도 ${matchPercentage}%</span>
                        </h3>
                        <p>${dest.desc}</p>
                        <div class="tags">
                            ${dest.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
                        </div>
                        <button class="select-btn" onclick="selectDestination(${index}, '${dest.name}', '${dest.flag}', '${dest.desc.replace(/'/g, "\\'")}', '${dest.tags.join(',')}')">
                            이 여행지로 선택하기
                        </button>
                    </div>
                `;
            });
            
            resultContent.innerHTML = html;
            
            // 결과를 전역 변수에 저장
            window.currentResults = results;
        }

        function selectDestination(index, name, flag, desc, tags) {
            // 결과 화면 숨기기
            document.getElementById('resultScreen').style.display = 'none';
            
            // 최종 선택 화면 보이기
            document.getElementById('finalChoice').classList.add('active');
            
            const tagsArray = tags.split(',');
            
            // 국가 좌표 가져오기
            const coords = countryCoordinates[name] || { lat: 0, lng: 0, zoom: 2 };
            
            const finalContent = document.getElementById('finalContent');
            finalContent.innerHTML = `
                <div class="final-card">
                    <div class="flag">${flag}</div>
                    <h2>${name}</h2>
                    <div class="congrats">당신의 최종 선택입니다!</div>
                </div>
                <div class="final-details">
                    <h3>🌟 여행지 정보</h3>
                    <p>${desc}</p>
                    <h3>✨ 주요 특징</h3>
                    <div class="tags">
                        ${tagsArray.map(tag => `<span class="tag">${tag}</span>`).join('')}
                    </div>
                </div>
                <div class="map-container">
                    <h3>🗺️ 위치 확인</h3>
                    <iframe 
                        class="map-frame"
                        src="https://maps.google.com/maps?q=${coords.lat},${coords.lng}&z=${coords.zoom}&output=embed"
                        loading="lazy"
                        referrerpolicy="no-referrer-when-downgrade">
                    </iframe>
                </div>
            `;
        }

        function restart() {
            currentQuestion = 0;
            answers = {};
            document.getElementById('resultScreen').style.display = 'none';
            document.getElementById('finalChoice').classList.remove('active');
            document.getElementById('startScreen').style.display = 'block';
            document.getElementById('progressFill').style.width = '0%';
        }
    </script>
</body>
</html>
