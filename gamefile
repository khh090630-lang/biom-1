import streamlit as st
import numpy as np
import matplotlib.pyplot as plt
import random

# ==========================================
# 0. 세션 상태 초기화 (Session State)
# ==========================================
def init_session_state():
    if 'score' not in st.session_state:
        st.session_state.score = 0
    if 'hp' not in st.session_state:
        st.session_state.hp = 100
    
    # siRNA 스나이퍼용 상태
    if 'target_mrna' not in st.session_state:
        st.session_state.target_mrna = ""
        
    # ASO 에디터용 상태
    if 'exon_error_idx' not in st.session_state:
        st.session_state.exon_error_idx = 0
        
    # 종합 모드용 상태
    if 'total_war_hp' not in st.session_state:
        st.session_state.total_war_hp = 100
    if 'current_crisis' not in st.session_state:
        st.session_state.current_crisis = None
    if 'crisis_msg' not in st.session_state:
        st.session_state.crisis_msg = ""
    if 'toxin_level' not in st.session_state:
        st.session_state.toxin_level = 50

init_session_state()

# 상보적 염기 변환 함수 (BioPython 로직 대체)
def get_complement(seq):
    comp_map = {'A': 'U', 'U': 'A', 'G': 'C', 'C': 'G'}
    return "".join([comp_map.get(base, base) for base in seq])

# 랜덤 mRNA 서열 생성기
def generate_random_mrna(length=6):
    return "".join(random.choices(['A', 'U', 'G', 'C'], k=length))

# ==========================================
# 1. UI 설정 및 메인 네비게이션
# ==========================================
st.set_page_config(page_title="Project Cure: Targeted Therapy", layout="wide")

st.sidebar.title("🏥 Project Cure")
st.sidebar.subheader("표적 치료 시뮬레이터")
menu = st.sidebar.radio("진료실 이동", ["[홈] 메인 대시보드", "🧬 RNA 치료 센터", "🧪 효소 치료 센터", "☣️ 종합 치료 (Total War)"])

st.sidebar.divider()
st.sidebar.metric(label="누적 연구 점수", value=f"{st.session_state.score} 점")

# ==========================================
# [홈] 메인 대시보드
# ==========================================
if menu == "[홈] 메인 대시보드":
    st.title("의생명공학 정밀 타격 시뮬레이션 플랫폼")
    st.markdown("현대 의학의 핵심인 **사후 억제(효소 저해)**와 **원천 차단(RNA 간섭)** 메커니즘을 체험하는 시뮬레이션입니다.")
    
    col1, col2, col3 = st.columns(3)
    
    with col1:
        st.info("### 🧬 RNA 치료\n**유전자 설계도의 정밀 타격**\n\n질병 단백질이 번역되기 전, mRNA 설계도를 파괴하거나 결함을 수선합니다.")
        st.caption("대표 약물: 파티시란(siRNA), 스핀라자(ASO)")
        
    with col2:
        st.success("### 🧪 효소 치료\n**단백질 활성 제어 및 정화**\n\n과활성된 효소 구조의 포켓을 막거나, 부족한 효소를 투입하여 대사 노폐물을 분해합니다.")
        st.caption("대표 약물: 리피토/네비라핀(저해제), 아스파라기나아제(추가)")
        
    with col3:
        st.error("### ☣️ 종합적 치료\n**복합 질환 디펜스 모드**\n\n모든 질병 상황이 동시다발적으로 발생합니다. 실시간으로 치료 무기를 교체하며 방어하세요.")
        st.caption("요구 역량: 메커니즘 판단력 및 순발력")

# ==========================================
# 🧬 RNA 치료 센터
# ==========================================
elif menu == "🧬 RNA 치료 센터":
    st.title("🧬 RNA 치료 센터")
    sub_mode = st.tabs(["🎯 siRNA 스나이퍼 (Patisiran 모델)", "✂️ ASO 에디터 (Spinraza 모델)"])
    
    # 1. siRNA 스나이퍼
    with sub_mode[0]:
        st.subheader("🎯 타겟: 간성 트랜스티레틴(TTR) 독성 mRNA 파괴")
        st.write("RISC 복합체를 가동하기 위해 타겟 mRNA의 **상보적 염기서열**을 정확히 입력하세요.")
        
        if not st.session_state.target_mrna:
            st.session_state.target_mrna = generate_random_mrna(6)
            
        st.markdown(f"### 내려오는 타겟 mRNA 서열:  `{st.session_state.target_mrna}`")
        answer = get_complement(st.session_state.target_mrna)
        
        user_input = st.text_input("siRNA 런처 장전 (상보 서열 대문자 입력):", key="sirna_input").upper()
        
        if st.button("🚀 발사 (RISC 유도)"):
            if user_input == answer:
                st.success("🎯 명중! RISC 복합체가 mRNA를 성공적으로 절단했습니다.")
                st.session_state.score += 20
                st.session_state.target_mrna = generate_random_mrna(6) # 다음 타겟 생성
                st.rerun()
            else:
                st.error(f"오조준! 올바른 상보 서열은 {answer} 입니다. 설계도가 빠져나갔습니다.")
                st.session_state.target_mrna = generate_random_mrna(6)
                st.rerun()

    # 2. ASO 에디터
    with sub_mode[1]:
        st.subheader("✂️ 타겟: SMN2 pre-mRNA 스플라이싱 수선 (엑손 스키핑/포함)")
        st.write("오류가 발생한 돌연변이 구간을 찾아 ASO 패치를 정확한 좌표에 붙이세요.")
        
        # 가상의 mRNA 가닥 (1~5번 구역)
        exons = ["정상 Exon 6", "정상 Exon 7", "오류 Exon 7b ⚠️", "정상 Exon 8"]
        if st.session_state.exon_error_idx == 0:
            st.session_state.exon_error_idx = 3 # 3번째가 오류
            
        col_e1, col_e2, col_e3, col_e4 = st.columns(4)
        cols = [col_e1, col_e2, col_e3, col_e4]
        for i, exon in enumerate(exons):
            with cols[i]:
                if "오류" in exon:
                    st.error(f"**구역 {i+1}**\n\n{exon}")
                else:
                    st.info(f"**구역 {i+1}**\n\n{exon}")
                    
        patch_target = st.radio("ASO 패치를 부착할 구역을 선택하세요:", [1, 2, 3, 4], horizontal=True)
        
        if st.button("🩹 패치 부착 및 스플라이싱 유도"):
            if patch_target == st.session_state.exon_error_idx:
                st.success("✨ 수선 완료! 돌연변이 엑손이 제외되고 정상 SMN 단백질 번역이 시작됩니다.")
                st.session_state.score += 30
                st.rerun()
            else:
                st.warning("잘못된 구역을 가렸습니다. 단백질 번역에 실패했습니다.")

# ==========================================
# 🧪 효소 치료 센터
# ==========================================
elif menu == "🧪 효소 치료 센터":
    st.title("🧪 효소 치료 센터")
    sub_mode = st.tabs(["🧩 효소 저해제 퍼즐 (Lipitor/Nevirapine)", "🧹 효소 추가 투입 (Asparaginase)"])
    
    # 1. 효소 저해제 퍼즐
    with sub_mode[0]:
        st.subheader("🧩 효소 활성 부위 및 올로스테릭 부위 공략 시뮬레이터")
        st.write("저해제의 타겟 포켓에 따른 미하엘리스-멘텐 동역학 변화를 분석합니다.")
        
        col_ctrl, col_graph = st.columns([1, 2])
        
        with col_ctrl:
            inhibitor_type = st.selectbox("약물 기전 선택 (도킹 포켓):", ["경쟁적 저해제 (활성 부위 도킹)", "비경쟁적 저해제 (올로스테릭 부위 도킹)"])
            i_conc = st.slider("저해제 투여 농도 [I]:", 0.0, 5.0, 1.0, step=0.5)
            
            # 수식 표현 (LaTeX 동역학 공식 사용)
            st.markdown("#### 효소 반응 속도 방정식")
            st.latex(r"v = \frac{V_{max}[S]}{K_m + [S]}")
            
            if "경쟁" in inhibitor_type:
                st.caption("기질과 직접 경쟁하므로 겉보기 **$K_m$이 증가**하여 그래프가 오른쪽으로 밀립니다. (리피토 모델)")
            else:
                st.caption("구조를 변형시켜 효소를 비활성화하므로 **$V_{max}$가 감소**합니다. (네비라핀 모델)")
                
        with col_graph:
            # 동역학 그래프 생성
            S = np.linspace(0, 10, 100)
            V_max = 10.0
            K_m = 2.0
            
            if "경쟁" in inhibitor_type:
                K_m_app = K_m * (1 + i_conc)
                V_max_app = V_max
            else:
                K_m_app = K_m
                V_max_app = V_max / (1 + i_conc)
                
            v_normal = (V_max * S) / (K_m + S)
            v_inhibited = (V_max_app * S) / (K_m_app + S)
            
            fig, ax = plt.subplots(figsize=(6, 4))
            ax.plot(S, v_normal, label="Normal Enzyme", color="blue", linestyle="--")
            ax.plot(S, v_inhibited, label=f"Inhibited (Type: {inhibitor_type[:3]})", color="red", linewidth=2)
            ax.set_title("Michaelis-Menten Kinetics")
            ax.set_xlabel("Substrate Concentration [S]")
            ax.set_ylabel("Reaction Velocity (v)")
            ax.set_ylim(0, 12)
            ax.legend()
            ax.grid(True, alpha=0.3)
            st.pyplot(fig)
            
        if st.button("🔗 약물 도킹 확정"):
            st.success(f"도킹 성공! 질병 효소의 활성이 제어되었습니다. (획득 점수: +15)")
            st.session_state.score += 15

    # 2. 효소 추가 투입
    with sub_mode[1]:
        st.subheader("🧹 백혈병 세포 영양분 고갈 작전 (아스파라기나아제 투여)")
        st.write("혈액 내 축적된 독성 아스파라긴 수치를 낮추어 종양 세포를 굶겨 죽이세요.")
        
        st.metric(label="현재 혈중 아스파라긴 농도", value=f"{st.session_state.toxin_level} mmol/L")
        st.progress(st.session_state.toxin_level / 100.0)
        
        col_btn1, col_btn2 = st.columns(2)
        with col_btn1:
            if st.button("💉 L-아스파라기나아제 10mg 투여"):
                st.session_state.toxin_level = max(0, st.session_state.toxin_level - 15)
                st.session_state.score += 10
                st.rerun()
        with col_btn2:
            if st.button("⏳ 시간 경과 (세포 대사 진행)"):
                st.session_state.toxin_level = min(100, st.session_state.toxin_level + 20)
                st.rerun()
                
        if st.session_state.toxin_level == 0:
            st.success("🎉 완벽한 대사 정화! 백혈병 세포가 사멸했습니다.")

# ==========================================
# ☣️ 종합 치료 (Total War Mode)
# ==========================================
elif menu == "☣️ 종합 치료 (Total War)":
    st.title("☣️ 종합 치료: 긴급 임상 방어 작전")
    st.markdown("환자의 체력이 실시간으로 소모됩니다. 돌발 질병 상황을 파악하고 알맞은 무기 번호를 선택해 위기를 넘기세요.")
    
    # 상단 환자 바이탈 사인
    col_hp, col_crisis = st.columns([1, 2])
    with col_hp:
        st.metric("환자 잔여 생명력 (Vital HP)", f"{st.session_state.total_war_hp} %")
        st.progress(st.session_state.total_war_hp / 100.0)
        
    with col_crisis:
        # 랜덤 위기 생성
        if st.session_state.current_crisis is None:
            crises = [
                {"id": 1, "name": "과다 mRNA 변이 생성", "weapon": 1, "desc": "간에서 독성 단백질 설계도가 쏟아져 나옵니다."},
                {"id": 2, "name": "pre-mRNA 스플라이싱 탈선", "weapon": 2, "desc": "중요한 엑손이 빠진 불량 설계도가 결합 중입니다."},
                {"id": 3, "name": "질병 효소 100% 과활성", "weapon": 3, "desc": "타겟 효소의 포켓이 활짝 열려 대사를 파괴합니다."},
                {"id": 4, "name": "혈액 내 종양 영양분 포화", "weapon": 4, "desc": "아스파라긴 수치가 급증하여 종양이 증식합니다."}
            ]
            st.session_state.current_crisis = random.choice(crises)
            st.session_state.crisis_msg = f"⚠️ [긴급 경보] {st.session_state.current_crisis['name']}\n\n증상: {st.session_state.current_crisis['desc']}"
            
        st.error(st.session_state.crisis_msg)
        
    st.divider()
    
    st.markdown("### 🛠️ 실시간 무기 교체 시스템 (적합한 대응책을 클릭하세요)")
    
    col_w1, col_w2, col_w3, col_w4 = st.columns(4)
    
    selected_weapon = 0
    with col_w1:
        if st.button("1️⃣ siRNA 스나이퍼\n(mRNA 타격)"): selected_weapon = 1
    with col_w2:
        if st.button("2️⃣ ASO 에디터\n(스플라이싱 교정)"): selected_weapon = 2
    with col_w3:
        if st.button("3️⃣ 효소 저해제\n(포켓 도킹 차단)"): selected_weapon = 3
    with col_w4:
        if st.button("4️⃣ 추가 효소 투여\n(독성 대사물 분해)"): selected_weapon = 4
        
    if selected_weapon != 0:
        if selected_weapon == st.session_state.current_crisis["weapon"]:
            st.success("🟢 정확한 처방입니다! 위기를 성공적으로 극복했습니다.")
            st.session_state.score += 50
            st.session_state.total_war_hp = min(100, st.session_state.total_war_hp + 5)
            st.session_state.current_crisis = None # 위기 초기화
            st.rerun()
        else:
            st.error("🔴 잘못된 치료제를 선택했습니다! 환자의 상태가 악화됩니다.")
            st.session_state.total_war_hp -= 15
            if st.session_state.total_war_hp <= 0:
                st.session_state.total_war_hp = 0
            st.rerun()
            
    if st.session_state.total_war_hp == 0:
        st.warning("💀 환자의 생명력이 0이 되었습니다. 연구 대시보드에서 체력을 초기화하세요.")
        if st.button("🔄 방어 시뮬레이션 다시 시작"):
            st.session_state.total_war_hp = 100
            st.session_state.current_crisis = None
            st.rerun()
