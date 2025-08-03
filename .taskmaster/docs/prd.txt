# AgriTune Studio (유튜브 생성기) 플랫폼 가이드: 개인용 PRD

## 목차
1. [제품 개요](#1-제품-개요)
2. [사용자 페르소나](#2-사용자-페르소나)
3. [핵심 기능 및 워크플로우](#3-핵심-기능-및-워크플로우)
4. [영상 템플릿 상세](#4-영상-템플릿-상세)
5. [기술 아키텍처](#5-기술-아키텍처)
6. [사용자 인터페이스](#6-사용자-인터페이스)
7. [데이터 관리](#7-데이터-관리)
8. [보안 및 개인정보보호](#8-보안-및-개인정보보호)
9. [성능 요구사항](#9-성능-요구사항)
10. [오류 처리 및 복구](#10-오류-처리-및-복구)
11. [개발 로드맵](#11-개발-로드맵)

---

## 1. 제품 개요

### 1.1 제품 정보
- **제품명**: AgriTune Studio (유튜브 생성기)
- **제품 유형**: AI 기반 영상 제작 플랫폼
- **개발 목표**: 농기계 튜닝 전문가를 위한 개인용 영상 제작 도구
- **핵심 가치**: 기술 전문성은 있지만 영상 제작이 어려운 1인 기술자를 위한 솔루션

### 1.2 제품 목표
1. **주요 목표**: 개인 기술자의 영상 제작 능력 향상
2. **부차 목표**: 
   - 영상 제작 시간 단축 (기존 8시간 → 30분)
   - 영상 제작 진입 장벽 제거
   - 개인 브랜딩 강화

### 1.3 핵심 기능
- **키워드 기반 아이디어 도출**: 간단한 키워드 입력으로 영상 아이디어 생성
- **AI 대본 작성**: Gemini를 활용한 전문적인 대본 자동 생성
- **영상 소스 제작**: Hailuo AI를 통한 맞춤형 영상 클립 생성
- **음성 합성**: ElevenLabs를 활용한 자연스러운 내레이션 생성
- **자동 편집**: 최종 유튜브 영상 완성까지 자동화
- **템플릿 커스터마이징**: 개인화된 영상 스타일 설정
- **재시도/이어하기**: 실패 지점부터 재시작 가능

---

## 2. 사용자 페르소나

### 2.1 주요 페르소나: 김기술 (35세)
```
이름: 김기술
나이: 35세
직업: 농기계 튜닝 전문가 (1인 기술자)
경력: 10년차
위치: 경기도 농촌 지역

특징:
- 농기계 튜닝 기술은 전문가 수준
- 영상 편집 경험 전무
- 유튜브 채널 운영 의향 있음
- 시간이 부족함 (하루 10-12시간 작업)
- 기술 공유에 관심이 많음

목표:
- 자신의 기술을 영상으로 공유
- 부수입 창출 (월 50-100만원)
- 지역 내 명성 확보

어려움:
- 영상 제작 방법을 모름
- 카메라 촬영 경험 없음
- 편집 소프트웨어 사용 불가
- 시간 투자 부담
```

### 2.2 보조 페르소나: 박농부 (42세)
```
이름: 박농부
나이: 42세
직업: 중소규모 농장 운영자
특징: 농기계 사용 경험 풍부, SNS 활동 적극적
목표: 농기계 관련 정보 공유, 농업 커뮤니티 형성
```

### 2.3 페르소나별 요구사항
| 페르소나 | 주요 요구사항 | 우선순위 |
|---------|-------------|---------|
| 김기술 | 간단한 영상 제작, 시간 절약 | 높음 |
| 박농부 | 정보 공유, 커뮤니티 형성 | 중간 |

---

## 3. 핵심 기능 및 워크플로우

### 3.1 전체 워크플로우
```
키워드 입력 → 농기계 모델 선택 → 템플릿 선택 → AI 처리 → 영상 생성 → 다운로드/업로드
```

### 3.2 입력 (Input) 단계

#### **3.2.1 필수 입력**
- **핵심 키워드**: 영상의 주요 메시지
  - 예시: "출력 25% 향상", "연비 15% 개선", "토크 30% 증가"
  - 형식: 구체적인 수치와 개선 효과 포함

#### **3.2.2 선택 입력**
- **대상 농기계 모델**: 구체적인 기종 지정
  - 예시: "존디어 6110M", "대동 트랙터 D500", "TYM T115"
  - 데이터베이스: 주요 농기계 모델 정보 보유

#### **3.2.3 템플릿 선택**
- **전후 비교형**: 성능 개선 효과 시각화
- **고객 인터뷰형**: 신뢰도 중심 스토리텔링
- **기술 설명형**: 전문성 강조
- **커스텀 템플릿**: 사용자 정의 템플릿

### 3.3 처리 (Processing) 단계

#### **3.3.1 아이디어 및 대본 생성 (Gemini)**
```python
# 대본 생성 로직 예시
def generate_script(keyword, machine_model, user_style):
    prompt = f"""
    농기계 튜닝 전문 유튜브 채널의 영상 대본을 작성해주세요.
    
    키워드: {keyword}
    대상 기종: {machine_model}
    사용자 스타일: {user_style}
    
    구조:
    1. [문제] - 현재 상황의 어려움
    2. [해결] - 튜닝 솔루션
    3. [결과] - 개선된 성능과 효과
    
    톤앤매너: 전문적이면서도 이해하기 쉬운 설명
    길이: 2-3분 영상에 적합한 분량
    """
    return gemini.generate(prompt)
```

#### **3.3.2 음성 합성 (ElevenLabs)**
- **음성 특성**: 신뢰감 있는 남성 목소리
- **톤앤매너**: 전문적이면서도 친근한 톤
- **속도**: 분당 150-180단어 (적정 속도)
- **개인화**: 사용자별 음성 설정 저장

#### **3.3.3 영상 소스 생성 (Hailuo AI)**
```python
# 영상 프롬프트 생성 예시
def generate_video_prompts(script, machine_model, user_preferences):
    prompts = []
    for scene in script.scenes:
        prompt = f"""
        농기계 튜닝 전문 영상의 장면입니다.
        
        장면: {scene.description}
        대상 기종: {machine_model}
        사용자 선호도: {user_preferences}
        스타일: 전문적, 고품질, 4K 해상도
        분위기: 신뢰감 있는 전문 서비스
        
        요구사항:
        - 농기계가 실제로 작동하는 모습
        - 전문적인 작업 환경
        - 성능 향상이 시각적으로 드러나는 장면
        """
        prompts.append(prompt)
    return prompts
```

### 3.4 출력 (Output) 단계

#### **3.4.1 최종 영상 구성**
- **해상도**: 1920x1080 (Full HD)
- **프레임레이트**: 30fps
- **길이**: 2-3분 (최적 길이)
- **포맷**: MP4 (H.264 코덱)

#### **3.4.2 필수 요소**
- **로고 워터마크**: 우측 하단 고정 (전체 재생 시간)
- **연락처 정보**: 마지막 5초 팝업
  - 전화번호: 010-4051-5179
  - 위치: 화면 중앙
  - 스타일: 눈에 띄는 디자인

---

## 4. 영상 템플릿 상세

### 4.1 전후 비교형 (Before & After)

#### **4.1.1 화면 구성**
```
┌─────────────────┬─────────────────┐
│   튜닝 전 상황   │   튜닝 후 결과   │
│   (좌측 화면)   │   (우측 화면)   │
└─────────────────┴─────────────────┘
```

#### **4.1.2 영상 흐름**
1. **도입부 (0-30초)**
   - 문제 상황 제시
   - 예시: "존디어 6110M으로 3헥타르 밭을 가는 데 4시간이 걸립니다"

2. **전환부 (30-60초)**
   - 튜닝박사 소개
   - 해결 과정 간략 설명

3. **비교부 (60-150초)**
   - 분할 화면으로 전후 비교
   - 성능 수치 강조 (자막)

4. **결론부 (150-180초)**
   - 최종 결과 요약
   - 연락처 정보

#### **4.1.3 AI 활용**
- **Gemini**: 성능 데이터 기반 비교 설명 대본
- **Hailuo AI**: 전후 상황 대비 영상 생성
- **ElevenLabs**: 객관적이고 신뢰감 있는 내레이션

### 4.2 고객 인터뷰형

#### **4.2.1 화면 구성**
```
┌─────────────────────────────────┐
│  AI 아바타 (농민) + 실제 영상   │
│  교차 편집                      │
└─────────────────────────────────┘
```

#### **4.2.2 영상 흐름**
1. **인터뷰 시작 (0-30초)**
   - AI 아바타 등장
   - 고객 소개 및 문제 상황 설명

2. **튜닝 과정 (30-90초)**
   - 실제 튜닝 작업 영상
   - 고객의 만족도 표현

3. **결과 확인 (90-150초)**
   - 튜닝된 기계 작동 영상
   - 성능 개선 효과 시연

4. **추천 메시지 (150-180초)**
   - AI 아바타의 추천 메시지
   - 연락처 정보

#### **4.2.3 AI 활용**
- **Gemini**: 자연스러운 인터뷰 대본
- **Hailuo AI**: 농민 아바타 및 작업 영상
- **ElevenLabs**: 구수하고 신뢰감 있는 목소리

### 4.3 기술 설명형

#### **4.3.1 화면 구성**
```
┌─────────────────────────────────┐
│  AI 엔지니어 아바타 + 다이어그램 │
│  전문적인 기술 설명              │
└─────────────────────────────────┘
```

#### **4.3.2 영상 흐름**
1. **문제 제기 (0-30초)**
   - 기술적 문제 설명
   - 예시: "존디어 6110M의 출력 저하 원인"

2. **기술 설명 (30-120초)**
   - ECU 맵핑 원리
   - 흡배기 튜닝 기술
   - 다이어그램과 차트 활용

3. **성능 예측 (120-150초)**
   - 개선 효과 데이터
   - 그래프로 시각화

4. **신뢰성 강조 (150-180초)**
   - 전문성 어필
   - 연락처 정보

#### **4.3.3 AI 활용**
- **Gemini**: 전문 용어와 기술 설명
- **Hailuo AI**: 엔지니어 아바타 및 다이어그램
- **ElevenLabs**: 명확하고 전문적인 톤

### 4.4 커스텀 템플릿

#### **4.4.1 개인화 기능**
- **자막 위치**: 사용자가 직접 설정
- **로고 크기**: 조절 가능
- **인트로/아웃트로**: 개인 문구 저장
- **색상 테마**: 브랜드 컬러 적용

#### **4.4.2 템플릿 저장**
```javascript
// 커스텀 템플릿 저장 예시
const customTemplate = {
    name: "김기술 스타일",
    subtitle_position: "bottom-center",
    logo_size: "medium",
    intro_text: "안녕하세요, 튜닝박사 김기술입니다",
    outro_text: "궁금한 점이 있으시면 언제든 연락주세요",
    color_theme: "#2E8B57"
}
```

---

## 5. 기술 아키텍처

### 5.1 시스템 구성
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   프론트엔드    │    │   백엔드 API    │    │   AI 서비스     │
│   (React/Vue)   │◄──►│   (Node.js)     │◄──►│   (Gemini/      │
│                 │    │                 │    │   ElevenLabs/   │
└─────────────────┘    └─────────────────┘    │   Hailuo AI)    │
                                              └─────────────────┘
                                              ┌─────────────────┐
                                              │   Supabase      │
                                              │   (PostgreSQL)  │
                                              └─────────────────┘
```

### 5.2 기술 스택
- **프론트엔드**: React.js, TypeScript, Tailwind CSS
- **백엔드**: Node.js, Express.js
- **데이터베이스**: Supabase (PostgreSQL)
- **AI 서비스**: 
  - Gemini API (대본 생성)
  - ElevenLabs API (음성 합성)
  - Hailuo AI API (영상 생성)
- **영상 처리**: FFmpeg, OpenCV
- **파일 저장**: Supabase Storage

### 5.3 데이터베이스 설계 (Supabase)
```sql
-- 사용자 테이블
CREATE TABLE users (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100),
    preferences JSONB,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 영상 프로젝트 테이블
CREATE TABLE projects (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    keyword VARCHAR(255) NOT NULL,
    machine_model VARCHAR(255),
    template_type VARCHAR(50) CHECK (template_type IN ('before_after', 'interview', 'technical', 'custom')),
    status VARCHAR(50) CHECK (status IN ('draft', 'processing', 'completed', 'failed', 'paused')),
    video_url VARCHAR(500),
    script_content TEXT,
    audio_url VARCHAR(500),
    progress JSONB,
    error_log TEXT,
    retry_count INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 커스텀 템플릿 테이블
CREATE TABLE custom_templates (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    template_config JSONB NOT NULL,
    is_default BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- 농기계 모델 테이블
CREATE TABLE machine_models (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    brand VARCHAR(100) NOT NULL,
    model VARCHAR(100) NOT NULL,
    category VARCHAR(100),
    specifications JSONB,
    tuning_options JSONB,
    performance_improvements JSONB,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- RLS (Row Level Security) 설정
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE projects ENABLE ROW LEVEL SECURITY;
ALTER TABLE custom_templates ENABLE ROW LEVEL SECURITY;
ALTER TABLE machine_models ENABLE ROW LEVEL SECURITY;

-- 사용자별 데이터 접근 정책
CREATE POLICY "Users can view own data" ON users FOR ALL USING (auth.uid() = id);
CREATE POLICY "Users can manage own projects" ON projects FOR ALL USING (auth.uid() = user_id);
CREATE POLICY "Users can manage own templates" ON custom_templates FOR ALL USING (auth.uid() = user_id);
CREATE POLICY "Users can view machine models" ON machine_models FOR SELECT USING (true);
```

### 5.4 Supabase 설정
```javascript
// Supabase 클라이언트 설정
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.SUPABASE_URL
const supabaseKey = process.env.SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseKey)

// 프로젝트 생성 예시
async function createProject(projectData) {
    const { data, error } = await supabase
        .from('projects')
        .insert([projectData])
        .select()
    
    if (error) throw error
    return data[0]
}

// 프로젝트 조회 예시
async function getProjects(userId) {
    const { data, error } = await supabase
        .from('projects')
        .select('*')
        .eq('user_id', userId)
        .order('created_at', { ascending: false })
    
    if (error) throw error
    return data
}
```

---

## 6. 사용자 인터페이스

### 6.1 메인 대시보드
```
┌─────────────────────────────────────────────────────────┐
│  AgriTune Studio                    [설정] [도움말]    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [새 프로젝트 생성]  [최근 프로젝트]  [통계]            │
│                                                         │
│  ┌─────────────────┐  ┌─────────────────┐              │
│  │   프로젝트 1    │  │   프로젝트 2    │              │
│  │   상태: 완료     │  │   상태: 처리중   │              │
│  └─────────────────┘  └─────────────────┘              │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 6.2 프로젝트 생성 화면
```
┌─────────────────────────────────────────────────────────┐
│  새 프로젝트 생성                                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  프로젝트 제목: [________________]                      │
│                                                         │
│  핵심 키워드: [________________]                        │
│  예시: "출력 25% 향상"                                 │
│                                                         │
│  대상 농기계: [드롭다운 선택]                           │
│  □ 존디어 6110M  □ 대동 D500  □ TYM T115              │
│                                                         │
│  템플릿 선택:                                           │
│  ○ 전후 비교형  ○ 고객 인터뷰형  ○ 기술 설명형          │
│  ○ 커스텀 템플릿 [설정]                                │
│                                                         │
│  [생성 시작]  [취소]                                    │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 6.3 프로젝트 상태 화면
```
┌─────────────────────────────────────────────────────────┐
│  프로젝트: 존디어 6110M 튜닝 효과                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  진행률: ████████████████████ 100%                     │
│                                                         │
│  단계별 상태:                                           │
│  ✅ 아이디어 생성 완료                                  │
│  ✅ 대본 작성 완료                                      │
│  ✅ 음성 합성 완료                                      │
│  ✅ 영상 생성 완료                                      │
│  ✅ 최종 편집 완료                                      │
│                                                         │
│  [미리보기]  [다운로드]  [Supabase 저장]  [재시도]     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 7. 데이터 관리

### 7.1 농기계 모델 데이터베이스 (Supabase)
```json
{
  "brand": "존디어",
  "model": "6110M",
  "category": "트랙터",
  "specifications": {
    "engine_power": "110hp",
    "weight": "4500kg",
    "fuel_type": "디젤",
    "year": "2020-2024"
  },
  "tuning_options": [
    "ECU 맵핑",
    "흡배기 튜닝",
    "연료 시스템 개선"
  ],
  "performance_improvements": {
    "power_increase": "25%",
    "fuel_efficiency": "15%",
    "torque_increase": "30%"
  }
}
```

### 7.2 Supabase 파일 관리
- **프로젝트 데이터**: Supabase PostgreSQL 데이터베이스
- **영상 파일**: Supabase Storage에 저장
- **백업**: Supabase 자동 백업 기능 활용

### 7.3 Supabase Storage 구조
```
agritune-studio/
├── projects/           # 프로젝트 폴더
│   ├── {project_id}/  # 프로젝트별 폴더
│   │   ├── script.txt # 생성된 대본
│   │   ├── audio.mp3  # 음성 파일
│   │   ├── video.mp4  # 최종 영상
│   │   └── assets/    # 임시 파일들
│   └── {project_id}/
├── templates/          # 템플릿 파일들
├── models/            # 농기계 모델 데이터
└── settings/          # 설정 파일들
```

### 7.4 Supabase 실시간 기능
```javascript
// 실시간 프로젝트 상태 업데이트
const subscription = supabase
    .channel('projects')
    .on('postgres_changes', 
        { event: '*', schema: 'public', table: 'projects' },
        (payload) => {
            console.log('프로젝트 상태 변경:', payload)
            updateProjectStatus(payload.new)
        }
    )
    .subscribe()
```

---

## 8. 보안 및 개인정보보호

### 8.1 Supabase 보안
- **Row Level Security (RLS)**: 사용자별 데이터 접근 제어
- **JWT 인증**: Supabase Auth 활용
- **API 키 관리**: 환경 변수로 안전한 관리

### 8.2 API 키 관리
- **환경 변수**: API 키를 환경 변수로 관리
- **Supabase Vault**: 민감한 정보 암호화 저장
- **접근 로그**: Supabase 로그 기능 활용

### 8.3 개인정보보호
- **최소 수집**: 필요한 정보만 수집
- **Supabase 저장**: 모든 데이터 Supabase에 저장
- **삭제 기능**: 언제든지 데이터 삭제 가능

---

## 9. 성능 요구사항

### 9.1 응답 시간
- **페이지 로딩**: 2초 이내
- **AI 처리**: 5분 이내 (영상 길이에 따라)
- **Supabase 쿼리**: 100ms 이내

### 9.2 시스템 요구사항
- **최소 사양**:
  - CPU: Intel i5 이상
  - RAM: 8GB 이상
  - 저장공간: 20GB 이상 (로컬 캐시용)
  - GPU: 영상 처리용 (선택사항)

### 9.3 네트워크 요구사항
- **인터넷 연결**: Supabase 및 AI 서비스 사용을 위해 필요
- **업로드 속도**: 영상 업로드용 10Mbps 이상
- **다운로드 속도**: AI 모델 다운로드용 50Mbps 이상

---

## 10. 오류 처리 및 복구

### 10.1 오류 메시지 정책
```javascript
// 구체적인 오류 메시지 예시
const errorMessages = {
    'ELEVENLABS_API_ERROR': {
        message: 'ElevenLabs API 키가 유효하지 않습니다.',
        solution: '설정에서 API 키를 확인하고 다시 시도해주세요.',
        action: '설정으로 이동'
    },
    'HAILUO_AI_ERROR': {
        message: '영상 생성 중 오류가 발생했습니다.',
        solution: '네트워크 연결을 확인하고 잠시 후 다시 시도해주세요.',
        action: '재시도'
    },
    'GEMINI_API_ERROR': {
        message: '대본 생성 중 오류가 발생했습니다.',
        solution: 'API 키를 확인하고 다시 시도해주세요.',
        action: '설정 확인'
    },
    'SUPABASE_CONNECTION_ERROR': {
        message: '데이터베이스 연결에 실패했습니다.',
        solution: '인터넷 연결을 확인하고 다시 시도해주세요.',
        action: '재연결'
    }
}
```

### 10.2 재시도/이어하기 기능
```javascript
// 프로젝트 상태 관리
const projectStates = {
    'draft': '초안',
    'processing': '처리중',
    'completed': '완료',
    'failed': '실패',
    'paused': '일시정지'
}

// 단계별 진행 상태
const processingSteps = [
    'script_generation',
    'audio_synthesis', 
    'video_generation',
    'final_editing'
]

// 실패 지점부터 재시작
async function resumeFromStep(projectId, step) {
    const { data, error } = await supabase
        .from('projects')
        .update({ 
            status: 'processing',
            progress: { current_step: step }
        })
        .eq('id', projectId)
    
    if (error) throw error
    return startProcessingFromStep(step)
}
```

### 10.3 자동 복구 시스템
- **네트워크 오류**: 자동 재연결 시도
- **API 오류**: 재시도 로직 (최대 3회)
- **부분 실패**: 성공한 단계는 유지하고 실패한 단계만 재시도
- **백업 저장**: 각 단계 완료 시 중간 결과 저장

---

## 11. 개발 로드맵

### 11.1 Phase 1 (1-2개월): 기본 기능 개발
- **기능**: 기본 영상 생성 기능
- **목표**: 개인 사용 가능한 MVP
- **산출물**: 데스크톱 애플리케이션 v1.0
- **주요 작업**:
  - 기본 UI/UX 구현
  - AI 서비스 연동
  - Supabase 데이터베이스 구축

### 11.2 Phase 2 (3-4개월): 기능 개선
- **기능**: 템플릿 추가, AI 성능 개선
- **목표**: 사용성 향상
- **산출물**: 데스크톱 애플리케이션 v2.0
- **주요 작업**:
  - 커스텀 템플릿 기능
  - 오류 처리 및 복구 시스템
  - 성능 최적화

### 11.3 Phase 3 (5-6개월): 고급 기능
- **기능**: 고급 편집 기능, 배치 처리
- **목표**: 효율성 극대화
- **산출물**: 완성된 개인용 도구
- **주요 작업**:
  - 배치 영상 생성
  - 고급 편집 옵션
  - 사용자 피드백 반영

### 11.4 Phase 4 (7-8개월): 확장 기능
- **기능**: 모바일 앱, API 제공
- **목표**: 접근성 향상
- **산출물**: 모바일 애플리케이션
- **주요 작업**:
  - 모바일 앱 개발
  - API 문서화
  - 커뮤니티 기능

---

## 부록

### A. Supabase 설치 및 설정
- **Supabase 계정 생성**: https://supabase.com
- **프로젝트 생성**: 새 프로젝트 생성 후 API 키 발급
- **환경 변수 설정**:
  ```bash
  SUPABASE_URL=your_supabase_url
  SUPABASE_ANON_KEY=your_supabase_anon_key
  ```

### B. 사용법
- **첫 실행**: Supabase 연결 및 API 키 설정 필요
- **프로젝트 생성**: 키워드 입력 → 모델 선택 → 템플릿 선택
- **결과 확인**: Supabase Storage에서 영상 확인

### C. 문제 해결
- **Supabase 연결 오류**: API 키 및 URL 확인
- **영상 생성 실패**: 네트워크 연결 확인
- **성능 문제**: 시스템 사양 확인
- **API 오류**: 각 서비스별 API 키 확인

---

**문서 버전**: v1.2  
**최종 수정일**: 2024년 12월  
**작성자**: AgriTune Studio 개발팀  
**용도**: 개인 사용 (Supabase 활용) 