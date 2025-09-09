
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>온라인 줄넘기 대회 관리 시스템</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        :root {
            --primary: #4a7a8c;
            --secondary: #6a8caf;
            --accent: #8c7a4a;
            --light: #f0f0f0;
            --dark: #333;
            --danger: #dc3545;
            --success: #28a745;
            --warning: #ffc107;
            --info: #17a2b8;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Malgun Gothic', '맑은 고딕', sans-serif;
        }
        
        body {
            background-color: var(--light);
            color: var(--dark);
            line-height: 1.6;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background-color: var(--primary);
            color: white;
            padding: 1rem;
            text-align: center;
            margin-bottom: 20px;
            border-radius: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .user-info {
            font-size: 0.9rem;
        }
        
        .logout-btn {
            background: var(--secondary);
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 3px;
            cursor: pointer;
        }
        
        .login-section {
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        
        .tab-container {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 2px solid var(--primary);
            flex-wrap: wrap;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background: #e0e0e0;
            margin-right: 5px;
            border-radius: 5px 5px 0 0;
            margin-bottom: 5px;
        }
        
        .tab.active {
            background: var(--primary);
            color: white;
        }
        
        .tab-content {
            display: none;
            background: white;
            padding: 极px;
            border-radius: 0 5px 5px 5px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        
        .tab-content.active {
            display: block;
        }
        
        form {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        @media (max-width: 768px) {
            form {
                grid-template-columns: 1fr;
            }
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        button {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        
        button:hover {
            background-color: var(--secondary);
        }
        
        button.danger {
            background-color: var(--danger);
        }
        
        button.success {
            background-color: var(--success);
        }
        
        button.warning {
            background-color: var(--warning);
            color: var(--dark);
        }
        
        button.info {
            background-color: var(--info);
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: var(--primary);
            color: white;
        }
        
        tr:hover {
            background-color: #f5极5f5;
        }
        
        .metrics {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .metric-card {
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 极 2px 10px rgba(0,0,0,0.1);
            text-align: center;
        }
        
        .metric-value {
            font-size: 24px;
            font-weight: bold;
            color: var(--primary);
        }
        
        footer {
            text-align: center;
            margin-top: 30px;
            padding: 20px;
            background: var(--dark);
            color: white;
            border-radius: 5px;
        }
        
        /* 모바일 대응 */
        @media (max-width: 768极) {
            .container {
                padding: 10px;
            }
            
            .tab {
                padding: 8px 12px;
                font-size: 14px;
            }
            
            th, td {
                padding: 8px 10px;
                font-size: 14px;
            }
            
            header {
                flex-direction: column;
                gap: 10px;
            }
        }
        
        .filter-section {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
        
        .badge {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
        }
        
        .badge-admin {
            background: var(--primary);
            color: white;
        }
        
        .badge-recorder {
            background: var(--secondary);
            color: white;
        }
        
        .import-section {
            background: #e8f4f8;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
        
        .file-upload {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .preview-table {
            max-height: 300px;
            overflow-y: auto;
            margin-top: 15px;
            border: 1px solid #ddd;
        }
        
        .instructions {
            background: #fff3cd;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 15px;
            border-left: 4px solid var(--warning);
        }
        
        .instructions h4 {
            margin-top: 0;
            color: var(--dark);
        }
        
        .instructions ul {
            margin-bottom: 0;
            padding-left: 20px;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: 5px;
            max-width: 500px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 1px solid #ddd;
            padding-bottom: 10px;
        }
        
        .close {
            font-size: 24px;
            cursor: pointer;
        }
        
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        .error-message {
            color: var(--danger);
            font-size: 14px;
            margin-top: 5px;
        }
        
        .test-account {
            margin-top: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 5px;
            border-left: 4px solid var(--info);
        }
        
        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 5px;
            color: white;
            z-index: 1000;
            display: none;
        }
        
        .notification.success {
            background-color: var(--success);
        }
        
        .notification.error {
            background-color: var(--danger);
        }
        
        .edit-form {
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 15px;
            border-left: 4px solid var(--info);
        }
    </style>
</head>
<body>
    <div class="notification" id="notification"></div>
    
    <div class="container">
        <header id="appHeader" style="display: none;">
            <h1>온라인 줄넘기 대회 관리 시스템</h1>
            <div class="user-info">
                <span id="currentUserInfo"></span>
                <button class极logout-btn" onclick="logout()">로그아웃</button>
            </div>
        </header>
        
        <section class="login-section" id="loginSection">
            <h2>로그인</h2>
            <form id="loginForm">
                <div class="form-group">
                    <label for="username">사용자명</label>
                    <input type="text" id="username" required>
                </div>
                <div class="form-group">
                    <label for="password">비밀번호</label>
                    <input type="password" id="password" required>
                </div>
                <button type="submit">로그인</button>
                <div id="loginError" class="error-message" style="display: none;"></div>
            </form>
            
            <div class="test-account">
                <h4>테스트 계정 정보</h4>
                <p><strong>관리자 계정:</strong> admin / admin123</p>
                <p><strong>기록담당자 계정:</strong> recorder / recorder123</p>
                <p>데모를 위해 위 계정으로 로그인하세요.</p>
            </div>
        </section>
        
        <div class="loading" id="loading">
            <p>데이터를 불러오는 중입니다...</p>
        </div>
        
        <main id="mainApp" style="display: none;">
            <div class="tab-container">
                <div class="tab active" data-tab="dashboard">대시보드</div>
                <div class="tab" data-tab="participants">참가자 관리</div>
                <div class="tab" data-tab="scores">경기 기록</div>
                <div class="tab" data-tab="rankings">순위 현황</div>
                <div class="tab" data-tab="criteria">시상 기준</div>
                <div class="tab" data-tab="users" id="usersTab">사용자 관리</div>
            </div>
            
            <div class="tab-content active" id="dashboard">
                <h2>대시보드</h2>
                <div class="metrics">
                    <极 class="metric-card">
                        <div class="metric-label">총 참가자</div>
                        <div class="metric-value" id="totalParticipants">0</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">총 경기수</div>
                        <div class="metric-value" id="totalMatches">0</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">총 수상자</div>
                        <div class="metric-value" id="totalWinners">0</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">활동 심판</div>
                        <div class="metric-value" id="activeJudges">0</div>
                    </div>
                </div>
                
                <h3>최근 시상 기록</h3>
                <table>
                    <thead>
                        <tr>
                            <th>번호</th>
                            <th>이름</th>
                            <th>종목</th>
                            <th>참가부</th>
                            <th>수상</th>
                            <th>소속</th>
                        </tr>
                    </thead>
                    <tbody id="recentAwards">
                        <!-- 최근 시상 기록이 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
            
            <div class="tab-content" id="participants">
                <h2>참가자 관리</h2>
                
                <div class="import-section">
                    <h3>엑셀 일괄 등록</h3>
                    <div class="instructions">
                        <h4>엑셀 파일 형식 안내</h4>
                        <ul>
                            <li>파일 형식: .xlsx 또는 .xls</li>
                            <li>열 순서: 번호, 이름, 소속, 참가부</li>
                            <li>첫 행은 헤더로 사용 (예: A1:번호, B1:이름, C1:소속, D1:참가부)</li>
                            <li>참가부: 유치부, 초등1부, 초등2부, 초등3부, 초등4부, 초등5부, 초등6부, 중등부, 고등부, 일반부, 선수</li>
                        </ul>
                    </div>
                    
                    <div class="file-upload">
                        <input type="file" id="excelFile" accept=".xlsx, .xls">
                        <button onclick="previewExcel()" class="info">미리보기</button>
                    </div>
                    
                    <div id="excelPreview" class="preview-table" style极display: none;">
                        <table>
                            <thead id="previewHeader">
                                <tr>
                                    <th>번호</th>
                                    <th>이름</th>
                                    <th>소속</th>
                                    <th>참가부</极>
                                    <th>상태</th>
                                </tr>
                            </thead>
                            <tbody id="previewBody">
                                <!-- 미리보기 데이터가 여기에 표시됩니다 -->
                            </tbody>
                        </table>
                    </div>
                    
                    <div class="action-buttons">
                        <button onclick="importExcel()" class="success" id="importBtn" style="display: none;">일괄 등록</button>
                        <button onclick="downloadTemplate()" class="warning">엑셀 템플릿 다운로드</button>
                    </div>
                </div>
                
                <h3>참가자 수동 등록</h3>
                <form id="participantForm">
                    <div class="form-group">
                        <label for="participantId">참가자 번호</label>
                        <input type="text" id="participantId" required>
                    </div>
                    <div class="form-group">
                        <label for="participantName">이름</label>
                        <input type="text" id="participantName" required>
                    </div>
                    <div class="form-group">
                        <label for="participantTeam">极속</label>
                        <input type="text" id="participantTeam" required>
                    </div>
                    <div class="form-group">
                        <label for="participantGrade">참가부</label>
                        <select id="participantGrade" required>
                            <option value="유치부">유치부</option>
                            <option value="초등1부">초등1부</option>
                            <option value="초등2부">초등2부</option>
                            <option value="초등3부">초등3부</option>
                            <option value="초등4부">초등4부</option>
                            <option value="초등5부">초등5부</option>
                            <option value="초등6부">초등6부</option>
                            <option value="중등부">중등부</option>
                            <option value="고등부">고등부</option>
                            <option value="일반부">일반부</option>
                            <option value="선수">선수</option>
                        </select>
                    </div>
                    <button type="submit">참가자 추가</button>
                </form>
                
                <h3>참가자 목록</h3>
                <div class="filter-section">
                    <div class="form-group">
                        <label for="participantFilterTeam">소속 필터</label>
                        <input type="text" id="participantFilterTeam" placeholder="소속명으로 필터">
                    </div>
                    <div class="form-group">
                        <label for="participantFilterGrade">참가부 필터</label>
                        <select id="participantFilterGrade">
                            <option value="">전체 부문</option>
                            <option value="유치부">유치부</option>
                            <option value="초등1부">초등1부</option>
                            <option value="초등2부">초등2부</option>
                            <option value="초등3부">초등3부</option>
                            <option value="초등4부">초등4부</option>
                            <option value="초등5부">초등5부</option>
                            <option value="초등6부">초등6부</option>
                            <option value="중등부">極등부</option>
                            <option value="고등부">고등부</option>
                            <option value="일반부">일반부</option>
                            <option value="선수">선수</option>
                        </select>
                    </div>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>번호</th>
                            <th>이름</th>
                            <th>소속</th>
                            <th>참가부</th>
                            <th>작업</th>
                        </tr>
                    </thead>
                    <tbody id="participantsList">
                        <!-- 참가자 목록이 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
            
            <div class="tab-content" id="scores">
                <h2>경기 기록 입력</h2>
                <form id="scoreForm">
                    <div class="form-group">
                        <label for="scoreParticipantId">참가자 번호</label>
                        <input type="text" id="scoreParticipantId" required>
                    </div>
                    <div class="form-group">
                        <label for="scoreEvent">경기 종목</label>
                        <select id="scoreEvent" required>
                            <option value="양발모아뛰기">양발모아뛰기</option>
                            <option value="30초 번갈아뛰기">30초 번갈아뛰기</option>
                            <option value="30초 이중뛰기">30초 이중뛰기</option>
                            <option value="2인 맞서뛰기1분">2인 맞서뛰기1분</option>
                            <option value="2인 스피드릴레이1분">2인 스피드릴레이1분</option>
                            <option value="2인번갈아뛰기">2인번갈아뛰기</option>
                            <option value="3중 뛰기">3중 뛰기</option>
                            <option value="8자마라톤">8자마라톤</option>
                            <option value="긴줄다함께뛰기">긴줄다함께뛰기</option>
                            <option value="가족3인 릴레이">가족3인 릴레이</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="scoreValue">점수</label>
                        <input type="number" id="scoreValue" step="0.1" required>
                    </div>
                    <button type="submit">기록 입력</button>
                </form>
                
                <h3>경기 기록</h3>
                <div class="filter-section">
                    <div class="form-group">
                        <label for="scoreFilterEvent">종목 필터</label>
                        <select id="scoreFilterEvent">
                            <option value="">전체 종목</option>
                            <option value="양발모아뛰기">양발모아뛰기</option>
                            <option value="30초 번갈아뛰기">30초 번갈아뛰기</option>
                            <option value="30초 이중뛰기">30초 이중뛰기</option>
                            <option value="2인 맞서뛰기1분">2인 맞서뛰기1분</option>
                            <option value="2인 스피드릴레이1분">2인 스피드릴레이1분</option>
                            <option value="2인번갈아뛰기">2인번갈아뛰기</option>
                            <option value极3중 뛰기">3중 뛰기</option>
                            <option value="8자마라톤">8자마라톤</option>
                            <option value="긴줄다함께뛰기">긴줄다함께뛰기</option>
                            <option value="가족3인 릴레이">가족3인 릴레이</option>
                        </select>
                    </div>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>참가자 번호</th>
                            <th>이름</th>
                            <th>경기 종목</th>
                            <th>참가부</th>
                            <th>점수</th>
                            <th>수상</th>
                            <th>작업</极>
                        </tr>
                    </thead>
                    <tbody id="scoresList">
                        <!-- 경기 기록이 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
            
            <div class="tab-content" id="rankings">
                <h2>순위 현황</h2>
                <div class="filter-section">
                    <div class="form-group">
                        <label for="rankingFilter">부문 필터</label>
                        <select id="rankingFilter">
                            <option value="">전체</option>
                            <option value="유치부">유치부</option>
                            <option value="초등1부">초등1부</option>
                            <option value="초등2부">초등2부</option>
                            <option value="초등3부">초등3부</option>
                            <option value="초등4부">초등4부</option>
                            <option value="초등5부">초등5부</option>
                            <option value="초등6부">초등6부</option>
                            <option value="중등부">중등부</option>
                            <option value="고등부">고등부</option>
                            <option value="일반부">일반부</option>
                            <option value="선수">선수</option>
                        </select>
                    </div>
                </div>
                
                <h3>개인 순위</h3>
                <table>
                    <thead>
                        <tr>
                            <th>순위</th>
                            <th>이름</th>
                            <th>소속</th>
                            <th>참가부</th>
                            <th>대상</th>
                            <th>금</th>
                            <th>은</th>
                            <极>동</th>
                            <th>총점</th>
                        </tr>
                    </thead>
                    <tbody id="individualRankings">
                        <!-- 개인 순위가 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
                
                <h3>단체 순위</h3>
                <table>
                    <thead>
                        <tr>
                            <th极순위</th>
                            <th>소속</th>
                            <th>대상</th>
                            <th>금</th>
                            <th>은</th>
                            <th>동</th>
                            <th>총점</th>
                        </tr>
                    </thead>
                    <tbody id="teamRankings">
                        <!-- 단체 순위가 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
            
            <div class="tab-content" id="criteria">
                <h2>시상 기준 관리</h2>
                <form id="criteriaForm">
                    <div class="form-group">
                        <label for="criteriaEvent">경기 종목</label>
                        <select id="criteria极vent" required>
                            <option value="양발모아뛰기">양발모아뛰기</option>
                            <option value="30초 번갈아뛰기">30초 번갈아뛰기</option>
                            <option value="30초 이중뛰기">30초 이중뛰기</option>
                            <option value="2인 맞서뛰기1분">2极 맞서뛰기1분</option>
                            <option value="2인 스피드릴레이1분">2인 스피드릴레이1분</option>
                            <option value="2인번갈아뛰기">2인번갈아뛰기</option>
                            <option value="3중 뛰기">3중 뛰기</option>
                            <option value="8자마라톤">8자마라톤</option>
                            <option value="긴줄다함께뛰기">긴줄다함께뛰기</option>
                            <option value="가족3인 릴레이">가족3인 릴레이</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="criteriaGrade">참가부</label>
                        <select id="criteriaGrade" required>
                            <option value="전체">전체 부문</option>
                            <option value极유치부">유치부</option>
                            <option value="초등1부">초등1부</option>
                            <option value="초등2부">초등2부</option>
                            <option value="초등3부">초등3부</option>
                            <option value="초등4부">초등4부</option>
                            <option value="초등5부">초등5부</option>
                            <option value="초등极부">초등6부</option>
                            <option value="중등부">중등부</option>
                            <option value="고등부">고등부</option>
                            <option value="일반부">일반부</option>
                            <option value="선수">선수</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="criteriaGold">금상 기준</label>
                        <input type="number" id="criteriaGold" step="0.1" required>
                    </div>
                    <div class极form-group">
                        <label for="criteriaSilver">은상 기준</label>
                        <input type极number" id="criteriaSilver" step="0.1" required>
                    </div>
                    <div class="form-group">
                        <label for="criteriaBronze">동상 기준</label>
                        <input type="number" id="criteriaBronze" step="0.1" required>
                    </div>
                    <button type="submit">기준 저장</button>
                </form>
                
                <h3>시상 기준 목록</h3>
                <div class="filter-section">
                    <div class="form-group">
                        <label for="criteriaFilterEvent">종목 필터</label>
                        <select id="criteriaFilterEvent">
                            <option value="">전체 종목</option>
                            <option value="양발모아뛰기">양발모아뛰기</option>
                            <option value="30초 번갈아뛰기">30초 번갈아뛰기</option>
                            <option value="30초 이중뛰기">30초 이중뛰기</option>
                            <option value="2인 맞서뛰기1분">2인 맞서뛰기1분</option>
                            <option value="2인 스피드릴레이1분">2인 스피드릴레이1분</option>
                            <option value="2인번갈아뛰기">2인번갈아뛰기</option>
                            <option value="3중 뛰기">3중 뛰기</option>
                            <option value="8자마라톤">8자마라톤</option>
                            <option value="긴줄다함께뛰기">긴줄다함께뛰기</option>
                            <option value="가족3인 릴레이">가족3인 릴레이</option>
                        </极elect>
                    </div>
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>경기 종목</th>
                            <th>참가부</th>
                            <th>금상 기준</th>
                            <th>은상 기준</th>
                            <th>동상 기준</th>
                            <th>작업</th>
                        </tr>
                    </thead>
                    <tbody id="criteriaList">
                        <!-- 시상 기준이 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
            
            <div class="tab-content" id="users">
                <h2>사용자 관리</h2>
                
                <!-- 사용자 수정 폼 (기본적으로 숨김) -->
                <div id="userEditForm" class="edit-form" style="display: none;">
                    <h3>사용자 정보 수정</h3>
                    <form id="editUserForm">
                        <input type="hidden" id="editUsername">
                        <div class="form-group">
                            <label for="editPassword">새 비밀번호</label>
                            <input type="password" id="editPassword">
                        </div>
                        <div class="form-group">
                            <label for="editUserRole">역할</label>
                            <select id="editUserRole" required>
                                <option value="admin">관리자</option>
                                <option value="recorder">기록 담당자(심판)</option>
                            </select>
                        </div>
                        <button type="submit" class="success">수정 완료</button>
                        <button type="button" onclick="cancelEditUser()" class="warning">취소</button>
                    </form>
                </div>
                
                <form id="userForm">
                    <div class="form-group">
                        <label for="newUsername">사용자명</label>
                        <input type="text" id="newUsername" required>
                    </div>
                    <div class="form-group">
                        <label for="newPassword">비밀번호</label>
                        <input type="password" id="newPassword" required>
                    </div>
                    <div class="form-group">
                        <label for="newUserRole">역할</label>
                        <select id="newUserRole" required>
                            <option value="admin">관리자</option>
                            <option value="recorder">기록 담당자(심판)</option>
                        </select>
                    </div>
                    <button type="submit">사용자 추가</button>
                </form>
                
                <h3>사용极 목록</h3>
                <table>
                    <thead>
                        <tr>
                            <th>사용자명</th>
                            <th>역할</th>
                            <th>작업</th>
                        </tr>
                    </thead>
                    <tbody id="usersList">
                        <!-- 사용자 목록이 여기에 동적으로 추가됩니다 -->
                    </tbody>
                </table>
            </div>
        </main>
        
        <!-- 모달 창 -->
        <div id="importModal" class="modal">
            <div class="modal-content">
                <div class="modal-header">
                    <h3>엑셀 일괄 등록 결과</h3>
                    <span class="close" onclick="closeModal()">&times;</span>
                </div>
                <div id="modalBody">
                    <!-- 모달 내용이 여기에 표시됩니다 -->
                </div>
                <div class="action-buttons">
                    <button onclick="closeModal()" class="success">확인</button>
                </div>
            </div>
        </div>
        
        <footer>
            <p>© 2024 대한 줄넘기협회 대회 관리 시스템. All rights reserved.</p>
        </footer>
    </div>

    <script>
        // 데이터 저장
        let participants = JSON.parse(localStorage.getItem('participants')) || [];
        let scores = JSON.parse(localStorage.getItem('scores')) || [];
        let results = JSON.parse(localStorage.getItem('results')) || [];
        let awardCriteria = JSON.parse(localStorage.getItem('awardCriteria')) || {};
        let users = JSON.parse(localStorage.getItem('users')) || [];
        let currentUser = null;
        let excelData = []; // 엑셀 미리보기 데이터 저장
        let editingUser = null; // 현재 수정 중인 사용자
        
        // 초기 데이터 설정
        function initializeData() {
            // 사용자 데이터가 없으면 기본 계정들 생성
            if (users.length === 0) {
                users = [
                    { username: 'admin', password: 'admin123', role: 'admin' },
                    { username: 'recorder', password: 'recorder123', role: 'recorder' }
                ];
                localStorage.setItem('users', JSON.stringify(users));
            } else {
                // 로컬 스토리지에서 사용자 데이터 로드
                users = JSON.parse(localStorage.getItem('users')) || [];
            }
            
            // 참가자 데이터가 없으면 샘플 데이터 생성
            if (participants.length === 0) {
                participants = [
                    { 번호: 'A1001', 이름: '김철수', 소속: '서울초등학교', 참가부: '초등1부' },
                    { 번호: 'A1002', 이름极'이영희', 소속: '서울초등학교', 참가부: '초등1부' },
                    { 번호: 'A1003', 이름: '박민수', 소속: '부산초등학교', 참가부: '초등2부' }
                ];
                localStorage.setItem('participants', JSON.stringify(participants));
            }
            
            // 시상 기준 데이터가 없으면 샘플 데이터 생성
            if (Object.keys(awardCriteria).length === 0) {
                awardCriteria = {
                    '양발모아뛰기': { 
                        '전체': { 금상: 90, 은상: 80, 동상: 70 },
                        '초등1부': { 금상: 85, 은상: 75, 동상: 65 },
                        '초등2부': { 금상: 90, 은상: 80, 동상: 70 }
                    },
                    '30초 번갈아뛰기': { 
                        '전체': { 금상: 85, 은상: 75, 동상: 65 },
                        '초등1부': { 금상: 80, 은상: 70, 동상: 60 },
                        '초등2부': { 금상: 85, 은상: 75, 동상: 65 }
                    }
                };
                localStorage.setItem('awardCriteria', JSON.stringify(awardCriteria));
            }
        }
        
        // 알림 표시
        function showNotification(message, type) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.style.display = 'block';
            
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3000);
        }
        
        // 로딩 표시
        function showLoading(show) {
            document.getElementById('loading').style.display = show ? 'block' : 'none';
        }
        
        // 로그인 처리
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            const errorElement = document.getElementById('loginError');
            
            // 입력값 검증
            if (!username || !password) {
                errorElement.textContent = '사용자명과 비밀번호를 모두 입력해주세요.';
                errorElement.style.display = 'block';
                return;
            }
            
            // 사용자 인증
            const user = users.find(u => u.username === username && u.password === password);
            
            if (user) {
                currentUser = user;
                document.getElementById('loginSection').style.display = 'none';
                document.getElementById('mainApp').style.display = 'block';
                document.getElementById('appHeader').style.display = 'flex';
                errorElement.style.display = 'none';
                
                // 현재 사용자 정보 표시
                document.getElementById('currentUserInfo').textContent = 
                    `${user.username} (${user.role === 'admin' ? '관리자' : '기록 담당자'})`;
                
                // 역할에 따른 접근 제어
                document.querySelectorAll('.tab').forEach(tab => {
                    tab.style.display = 'block';
                });
                
                if (user.role === 'recorder') {
                    // 기록 담당자는 참가자 관리, 시상 기준, 사용자 관리 탭 숨기기
                    document.querySelectorAll('.tab').forEach(tab => {
                        if (tab.dataset.tab === 'participants' || 
                            tab.dataset.tab === 'criteria' || 
                            tab.dataset.tab === 'users') {
                            tab.style.display = 'none';
                        }
                    });
                }
                
                showLoading(true);
                setTimeout(() => {
                    loadAllData();
                    showLoading(false);
                    showNotification('로그인 성공!', 'success');
                }, 500);
            } else {
                errorElement.textContent = '사용자명 또는 비밀번호가 올바르지 않습니다.';
                errorElement.style.display = 'block';
                document.getElementById('password').value = '';
            }
        });
        
        // 로그아웃
        function logout() {
            currentUser = null;
            document.getElementById('loginSection').style.display = 'block';
            document.getElementById('mainApp').style.display = 'none';
            document.getElementById('appHeader').style.display = 'none';
            document.getElementById('loginForm').reset();
            document.getElementById('loginError').style.display = 'none';
            
            // 모든 탭 다시 보이도록 초기화
            document.querySelectorAll('.tab').forEach(tab => {
                tab.style.display = 'block';
            });
            
            showNotification('로그아웃 되었습니다.', 'success');
        }
        
        // 탭 전환
        document.querySelectorAll('.tab').forEach(tab => {
            tab.addEventListener('click', function() {
                // 모든 탭 비활성화
                document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                
                // 현재 탭 활성화
                this.classList.add('active');
                document.getElementById(this.dataset.tab).classList.add('active');
            });
        });
        
        // 엑셀 미리보기
        function previewExcel() {
            const fileInput = document.getElementById('excelFile');
            const file = fileInput.files[0];
            
            if (!file) {
                showNotification('엑셀 파일을 선택해주세요.', 'error');
                return;
            }
            
            const reader = new FileReader();
            reader.onload = function(e) {
                const data = new Uint8Array(e.target.result);
                const workbook = XLSX.read(data, { type: 'array' });
                
                // 첫 번째 시트 사용
                const firstSheet = workbook.Sheets[workbook.SheetNames[0]];
                const jsonData = XLSX.utils.sheet极json(firstSheet, { header: 1 });
                
                // 헤더 확인 (첫 번째 행)
                if (jsonData.length < 2) {
                    showNotification('엑셀 파일에 데이터가 없습니다.', 'error');
                    return;
                }
                
                const headers = jsonData[0];
                if (headers.length < 4 || 
                    headers[0] !== '번호' || 
                    headers[1] !== '이름' || 
                    headers[2] !== '소속' || 
                    headers[3] !== '참가부') {
                    showNotification('엑셀 파일 형식이 맞지 않습니다. 번호, 이름, 소속, 참가부 순서로 되어있는지 확인해주세요.', 'error');
                    return;
                }
                
                // 데이터 추출 (헤더 제외)
                excelData = [];
                const previewBody = document.getElementById('previewBody');
                previewBody.innerHTML = '';
                
                for (let i = 1; i < jsonData.length; i++) {
                    const row = jsonData[i];
                    if (row.length >= 4) {
                        const participant = {
                            번호: row[0] ? row[0].toString().trim() : '',
                            이름: row[1] ? row[1].toString().trim() : '',
                            소속: row[2] ? row[2].toString().trim() : '',
                            참가부: row[3] ? row[3].toString().trim() : ''
                        };
                        
                        excelData.push(participant);
                        
                        // 미리보기 테이블에 행 추가
                        const tr = document.createElement('tr');
                        
                        // 데이터 유효성 검사
                        let isValid = true;
                        let status = '✅ 유효';
                        
                        if (!participant.번호 || !participant.이름 || !participant.소속 || !participant.참가부) {
                            isValid = false;
                            status = '❌ 필수값 누락';
                        } else if (participants.some(p => p.번호 === participant.번호)) {
                            isValid = false;
                            status = '⚠️ 중복 번호';
                        }
                        
                        const gradeOptions = ["유치부", "초등1부", "초등2부", "초등3부", "초등4부", 
                                            "초등5부", "초등6부", "중등부", "고등부", "일반부", "선수"];
                        if (!gradeOptions.includes(participant.참가부)) {
                            isValid = false;
                            status = '❌ 잘못된 참가부';
                        }
                        
                        tr.innerHTML = `
                            <td>${participant.번호}</td>
                            <td>${participant.이름}</td>
                            <td>${participant.소속}</td>
                            <td>${participant.참가부}</td>
                            <td>极{status}</td>
                        `;
                        
                        if (!isValid) {
                            tr.style.backgroundColor = '#ffebee';
                        }
                        
                        previewBody.appendChild(tr);
                    }
                }
                
                // 미리보기 표시
                document.getElementById('excelPreview').style.display = 'block';
                document.getElementById('importBtn').style.display = 'inline-block';
            };
            
            reader.readAsArrayBuffer(file);
        }
        
        // 엑셀 데이터 일괄 등록
        function importExcel() {
            if (excelData.length === 0) {
                showNotification('등록할 데이터가 없습니다.', 'error');
                return;
            }
            
            let successCount = 0;
            let errorCount = 0;
            const errorMessages = [];
            
            // 기존 참가자 번호 목록
            const existingNumbers = participants.map(p => p.번호);
            
            // 참가부 옵션
            const gradeOptions = ["유치부", "초등1부", "초등2부", "초등3부", "초등4부", 
                                "초등5부", "초등6부", "중등부", "고등부", "일반부", "선수"];
            
            excelData.forEach((participant, index) => {
                // 데이터 유효성 검사
                if (!participant.번호 || !participant.이름 || !participant.소속 || !participant.참가부极 {
                    errorCount++;
                    errorMessages.push(`${index + 1}행: 필수값이 누락되었습니다.`);
                    return;
                }
                
                if (existingNumbers.includes(participant.번호)) {
                    error极ount++;
                    errorMessages.push(`${index + 1}행: 이미 존재하는 참가자 번호입니다. (${participant.번호})`);
                    return;
                }
                
                if (!gradeOptions.includes(participant.참가부)) {
                    errorCount++;
                    errorMessages.push(`${index + 1}행: 잘못된 참가부입니다. (${participant.참가부})`);
                    return;
                }
                
                // 참가자 추가
                participants.push(participant);
                successCount++;
            });
            
            // 데이터 저장
            saveData();
            
            // 결과 모달 표시
            const modalBody = document.getElementById('modal极ody');
            modalBody.innerHTML = `
                <p>총 ${excelData.length}명 중 ${successCount}명이 성공적으로 등록되었습니다.</p>
                ${errorCount > 0 ? `<p>${errorCount}명의 등록에 실패했습니다.</p>` : ''}
                ${errorMessages.length > 0 ? `
                    <div style="max-height: 200px; overflow-y: auto; margin-top: 10px;">
                        <h4>에러 목록:</h4>
                        <ul>
                            ${errorMessages.map(msg => `<li>${msg}</li>`).join('')}
                        </ul>
                    </div>
                ` : ''}
            `;
            
            document.getElementById('importModal').style.display = 'flex';
            
            // 목록 새로고침
            loadParticipants();
            updateDashboard();
            
            // 미리보기 초기화
            document.getElementById('excelPreview').style.display = 'none';
            document.getElementById('importBtn').style.display = 'none';
            document.getElementById('excelFile').value = '';
            excelData = [];
            
            showNotification(`${successCount}명의 참가자가 등록되었습니다.`, 'success');
        }
        
        // 엑셀 템플릿 다운로드
        function downloadTemplate() {
            // 샘플 데이터 생성
            const sampleData = [
                ['번호', '이름', '소속', '참가부'],
                ['A1001', '김철수', '서울초등학교', '초등1부'],
                ['A1002', '이영희', '서울초등학교', '초등1부'],
                ['A1003', '박민수', '부산초등학교', '초등2부'],
                ['A1004', '최지우', '대구초등학교', '초등3부']
            ];
            
            // 워크북 생성
            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.aoa_to_sheet(sampleData);
            
            // 열 너비 설정
            const colWidths = [
                { wch: 10 }, // 번호
                { wch: 15 }, // 이름
                { wch: 20 }, // 소속
                { wch: 15 }  // 참가부
            ];
            ws['!cols'] = colWidths;
            
            // 시트 추가
            XLSX.utils.book_append_sheet(wb, ws, '참가자목록');
            
            // 파일 다운로드
            XLSX.writeFile(wb, '줄넘기_참가자_템플릿.xlsx');
            
            showNotification('엑셀 템플릿이 다운로드되었습니다.', 'success');
        }
        
        // 모달 닫기
        function closeModal() {
            document.getElementById('importModal').style.display = 'none';
        }
        
        // 참가자 추가
        document.getElementById('participantForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const id = document.getElementById('participantId').value;
            const name = document.getElementById('participantName').value;
            const team = document.getElementById('participantTeam').value;
            const grade = document.getElementById('participantGrade').value;
            
            // 중복 확인
            if (participants.some(p => p.번호 === id)) {
                showNotification('이미 존재하는 참가자 번호입니다.', 'error');
                return;
            }
            
            participants.push({
                번호: id,
                이름: name,
                소속: team,
                참가부: grade
            });
            
            saveData();
            loadParticipants();
            updateDashboard();
            this.reset();
            
            showNotification('참가자가 추가되었습니다.', 'success');
        });
        
        // 경기 기록 입력
        document.getElementById('scoreForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const participantId = document.getElementById('scoreParticipantId').value;
            const event = document.getElementById('scoreEvent').value;
            const scoreValue = parseFloat(document.getElementById('scoreValue').value);
            
            // 참가자 확인
            const participant = participants.find(p => p.번호 === participantId);
            if (!participant) {
                showNotification('존재하지 않는 참가자 번호입니다.', 'error');
                return;
            }
            
            // 참가부에 맞는 시상 기준 찾기
            const grade = participant.참가부;
            let criteria = {};
            
            if (awardCriteria[event] && awardCriteria[event][grade]) {
                criteria = awardCriteria[event][grade];
            } else if (awardCriteria[event] && awardCriteria[event]['전체']) {
                criteria = awardCriteria[event]['전체'];
            }
            
            // 수상 등급 결정
            let award = '-';
            
            if (criteria.금상 && scoreValue >= criteria.금상) {
                // 금상 이상 상위 3명을 대상으로 선정
                const eventScores = scores.filter(s => s.경기종목 === event && s.점수 >= criteria.금상);
                const topScores = [...new Set(eventScores.map(s => s.점수))].sort((a, b) => b - a).slice(0, 3);
                
                if (topScores.includes(scoreValue)) {
                    award = '대상';
                } else {
                    award = '금상';
                }
            } else if (criteria.은상 && scoreValue >= criteria.은상) {
                award = '은상';
            } else if (criteria.동상 && scoreValue >= criteria.동상) {
                award = '동상';
            }
            
            const scoreId = scores.length > 0 ? Math.max(...scores.map(s => s.ID)) + 1 : 1;
            
            scores.push({
                ID: score极,
                참가자번호: participantId,
                경기종목: event,
                점수: scoreValue,
                수상: award
            });
            
            // 수상 결과 추가
            if (award !== '-') {
                results.push({
                    번호: participantId,
                    경기종목: event,
                    수상: award
                });
            }
            
            saveData();
            loadScores();
            updateDashboard();
            updateRankings();
            this.reset();
            
            showNotification('경기 기록이 입력되었습니다.', 'success');
        });
        
        // 시상 기준 저장
        document.getElementById('criteriaForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const event = document.getElementById('criteriaEvent').value;
            const grade = document.getElementById('criteriaGrade').value;
            const gold = parseFloat(document.getElementById('criteriaGold').value);
            const silver = parseFloat(document.getElementById('criteriaSilver').value);
            const bronze = parseFloat(document.getElementById('criteriaBronze').value);
            
            if (!awardCriteria[event]) {
                awardCriteria[event] = {};
            }
            
            awardCriteria[event][grade] = {
                금상: gold,
                은상: silver,
                동상: bronze
            };
            
            saveData();
            loadCriteria();
            this.reset();
            
            showNotification('시상 기준이 저장되었습니다.', 'success');
        });
        
        // 사용자 추가
        document.getElementById('userForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const username = document.getElementById('newUsername').value;
            const password = document.getElementById('newPassword').value;
            const role = document.getElementById('newUserRole').value;
            
            // 중복 확인
            if (users.some(u => u.username === username)) {
                showNotification('이미 존재하는 사용자명입니다.', 'error');
                return;
            }
            
            users.push({
                username: username,
                password: password,
                role: role
            });
            
            saveData();
            loadUsers();
            this.reset();
            
            showNotification('사용자가 추가되었습니다.', 'success');
        });
        
        // 사용자 수정 폼 제출
        document.getElementById('editUserForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const username = document.getElementById('editUsername').value;
            const password = document.getElementById('editPassword').value;
            const role = document.getElementById('editUserRole').value;
            
            // 사용자 찾기
            const userIndex = users.findIndex(u => u.username === username);
            if (userIndex === -1) {
                showNotification('사용자를 찾을 수 없습니다.', 'error');
                return;
            }
            
            // 비밀번호가 입력된 경우에만 업데이트
            if (password) {
                users[userIndex].password = password;
            }
            
            // 역할 업데이트
            users[userIndex].role = role;
            
            save极ata();
            loadUsers();
            cancelEditUser();
            
            showNotification('사용자 정보가 수정되었습니다.', 'success');
        });
        
        // 사용자 수정 취소
        function cancelEditUser() {
            document.getElementById('userEditForm').style.display = 'none';
            document.getElementById('userForm').style.display = 'grid';
            editingUser = null;
            document.getElementById('editUserForm').reset();
        }
        
        // 사용자 수정
        function editUser(username) {
            const user = users.find(u => u.username === username);
            if (!user) {
                showNotification('사용자를 찾을 수 없습니다.', 'error');
                return;
            }
            
            editingUser = username;
            document.getElementById('editUsername').value = user.username;
            document.getElementById('editUserRole').value = user.role;
            
            // 폼 표시 전환
            document.getElementById('userForm').style.display = 'none';
            document.getElementById('userEditForm').style.display = 'block';
        }
        
        // 사용자 삭제
        function deleteUser(username) {
            if (confirm('정말로 이 사용자를 삭제하시겠습니까?')) {
                // 현재 로그인한 사용자는 삭제 불가
                if (currentUser && currentUser.username === username) {
                    showNotification('현재 로그인한 사용자는 삭제할 수 없습니다.', 'error');
                    return;
                }
                
                users = users.filter(u => u.username !== username);
                saveData();
                loadUsers();
                
                showNotification('사용자가 삭제되었습니다.', 'success');
            }
        }
        
        // 데이터 저장
        function saveData() {
            localStorage.setItem('participants', JSON.stringify(participants));
            localStorage.setItem('scores', JSON.stringify(scores));
            localStorage.setItem('results', JSON.stringify(results));
            localStorage.setItem('awardCriteria', JSON.stringify(awardCriteria));
            localStorage.setItem('users', JSON.stringify(users));
        }
        
        // 모든 데이터 로드
        function loadAllData() {
            loadParticipants();
            loadScores();
            loadCriteria();
            loadUsers();
            updateDashboard();
            updateRankings();
        }
        
        // 참가자 목록 로드
        function loadParticipants() {
            const tbody = document.getElementById('participantsList');
            tbody.innerHTML = '';
            
            const teamFilter = document.getElementById('participantFilterTeam').value.toLowerCase();
            const gradeFilter = document.getElementById('participantFilterGrade').value;
            
            let filteredParticipants = participants;
            
            // 필터 적용
            if (teamFilter) {
                filteredParticipants = filteredParticipants.filter(p => 
                    p.소속.toLowerCase().includes(teamFilter)
                );
            }
            
            if (gradeFilter) {
                filteredParticipants = filteredParticipants.filter(p => 
                    p.참가부 === gradeFilter
                );
            }
            
            filteredParticipants.forEach(p => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${p.번호}</td>
                    <td>${p.이름}</td>
                    <td>${p.소속}</td>
                    <td>${p.참가부}</td>
                    <td>
                        ${currentUser && currentUser.role === 'admin' ? 
                        `<button onclick="editParticipant('${p.번호}')">수정</button>
                         <button class="danger" onclick="deleteParticipant('${p.번호}')">삭제</button>` : 
                        ''}
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }
        
        // 경기 기록 로드
        function loadScores() {
            const tbody = document.getElementById('scoresList');
            tbody.innerHTML = '';
            const eventFilter = document.getElementById('scoreFilterEvent').value;
            
            let filteredScores = scores;
            if (eventFilter) {
                filteredScores = scores.filter(s => s.경기종목 === eventFilter);
            }
            
            filteredScores.forEach(s => {
                const participant = participants.find(p => p.번호 === s.참가자번호);
                const name = participant ? participant.이름 : '알 수 없음';
                const grade = participant ? participant.참가부 : '알 수 없음';
                
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${s.참가자번호}</td>
                    <td>${name}</td>
                    <td>${s.경기종목}</td>
                    <td>${grade}</td>
                    <td>${s.점수}</td>
                    <td>${s.수상}</td>
                    <td>
                        ${currentUser && currentUser.role === 'admin' ? 
                        `<button onclick="editScore(${s.ID})">수정</button>
                         <button class="danger" onclick="deleteScore(${s.ID})">삭제</button>` : 
                        ''}
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }
        
        // 시상 기준 로드
        function loadCriteria() {
            const tbody = document.getElementById('criteriaList');
            tbody.innerHTML = '';
            const eventFilter = document.getElementById('criteriaFilterEvent').value;
            
            for (const event in awardCriteria) {
                if (eventFilter && event !== eventFilter) continue;
                
                for (const grade in awardCriteria[event]) {
                    const criteria = awardCriteria[event][grade];
                    const tr = document.createElement('tr');
                    tr.innerHTML = `
                        <td>${event}</td>
                        <td>${grade}</td>
                        <td>${criteria.금상 || ''}</td>
                        <td>${criteria.은상 || ''}</td>
                        <td>${criteria.동상 || ''}</td>
                        <td>
                            ${currentUser && currentUser.role === 'admin' ? 
                            `<button onclick="editCriteria('${event}', '${grade}')">수정</button>
                             <button class="danger" onclick="deleteCriteria('${event}', '极rade}')">삭제</button>` : 
                            ''}
                        </td>
                    `;
                    tbody.appendChild(tr);
                }
            }
        }
        
        // 사용자 목록 로드
        function loadUsers() {
            const tbody = document.getElementById('usersList');
            tbody.innerHTML = '';
            
            users.forEach(u => {
                // 현재 로그인한 사용자는 삭제 불가
                const isCurrentUser = currentUser && u.username === currentUser.username;
                
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${u.username} ${isCurrentUser ? '(현재 사용자)' : ''}</td>
                    <td><span class="badge ${u.role === 'admin' ? 'badge-admin' : 'badge-recorder'}">${u.role === 'admin' ? '관리자' : '기록 담당자'}</span></td>
                    <td>
                        ${currentUser && currentUser.role === 'admin' ? 
                        `<button onclick="editUser('${u.username}')">수정</button>
                         ${!isCurrentUser ? `<button class="danger" onclick="deleteUser('${u.username}')">삭제</button>` : ''}` : 
                        ''}
                    </td>
                `;
                tbody.appendChild(tr);
            });
            
            // 활성 심판 수 업데이트
            const activeJudges = users.filter(u => u.role === 'recorder').length;
            document.getElementById('activeJudges').textContent = activeJudges;
        }
        
        // 대시보드 업데이트
        function updateDashboard() {
            document.getElementById('totalParticipants').textContent = participants.length;
            document.getElementById('totalMatches').textContent = scores.length;
            document.getElementById('totalWinners').textContent = results.length;
            
            const tbody = document.getElementById('recentAwards');
            tbody.innerHTML = '';
            
            // 최근 10개의 수상 결과
            const recentResults = results.slice(-10).reverse();
            
            recentResults.forEach(r => {
                const participant = participants.find(p => p.번호 === r.번호);
                if (participant) {
                    const tr = document.createElement('tr');
                    tr.innerHTML = `
                        <td>${participant.번호}</td>
                        <td>${participant.이름}</td>
                        <td>${r.경기종목}</td>
                        <td>${participant.참가부}</td>
                        <td>${r.수상}</td>
                        <td>${participant.소속}</td>
                    `;
                    tbody.appendChild(tr);
                }
            });
        }
        
        // 순위 업데이트
        function updateRankings() {
            const gradeFilter = document.getElementById('rankingFilter').value;
            
            // 개인 순위 계산
            const individualStats = {};
            
            results.forEach(r => {
                const participant = participants.find(p => p.번호 === r.번호);
                if (participant && (!gradeFilter || participant.참가부 === gradeFilter)) {
                    if (!individualStats[participant.이름]) {
                        individualStats[participant.이름] = {
                            이름: participant.이름,
                            소속: participant.소속,
                            참가부: participant.참가부,
                            대상: 0,
                            금: 0,
                            은: 0,
                            동: 0,
                            총점: 0
                        };
                    }
                    
                    if (r.极상 === '대상') individualStats[participant.이름].대상++;
                    else if (r.수상 === '금상') individualStats[participant.이름].금++;
                    else if (r.수상 === '은상') individualStats[participant.이름].은++;
                    else if (r.수상 === '동상') individualStats[participant.이름].동++;
                    
                    // 총점 계산 (대상:5, 금상:3, 은상:2, 동상:1)
                    individualStats[participant.이름].총점 = 
                        individualStats[participant.이름].대상 * 5 +
                        individualStats[participant.이름].금 * 3 +
                        individualStats[participant.이름].은 * 2 +
                        individualStats[participant.이름].동 * 1;
                }
            });
            
            // 개인 순위 정렬 및 표시
            const individualRankings = Object.values(individual极tats).sort((a, b) => b.총점 - a.총점);
            const individualTbody = document.getElementById('individualRankings');
            individualTbody.innerHTML = '';
            
            individualRankings.forEach((p, i) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${i + 1}</td>
                    <td>${p.이름}</td>
                    <td>${p.소속}</td>
                    <td>${p.참가부}</td>
                    <td>${p.대상}</td>
                    <td>${极.금}</td>
                    <td>${p.은}</td>
                    <td>${p.동}</td>
                    <td>${p.총점}</td>
                `;
                individualTbody.appendChild(tr);
            });
            
            // 단체 순위 계산
            const teamStats = {};
            
            results.forEach(r => {
                const participant = participants.find(p => p.번호 === r.번호);
                if (participant && (!gradeFilter || participant.참가부 === gradeFilter)) {
                    if (!teamStats[participant.소속]) {
                        teamStats[participant.소속] = {
                            소속: participant.소속,
                            대상: 0,
                            금: 0,
                            은: 0,
                            동: 0,
                            총점: 极
                        };
                    }
                    
                    if (r.수상 === '대상') teamStats[participant.소속].대상++;
                    else if (r.수상 === '금상') teamStats[participant.소속].금++;
                    else if (r.수상 === '은상') teamStats[participant.소속].은++;
                    else if (r.수상 === '동상') teamStats[participant.소속].동++;
                    
                    // 총점 계산
                    teamStats[participant.소속].총점 = 
                        teamStats[participant.소속].대상 * 5 +
                        teamStats[participant.소속].금 * 3 +
                        teamStats[participant.소속].은 * 2 +
                        teamStats[participant.소속].동 * 1;
                }
            });
            
            // 단체 순위 정렬 및 표시
            const teamRankings = Object.values(teamStats).sort((a, b) => b.총점 - a.총점);
            const teamTbody = document.getElementById('teamRankings');
            teamTbody.innerHTML = '';
            
            teamRankings.forEach((t, i) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${i + 1}</td>
                    <td>${t.소속}</td>
                    <td>${t.대상}</td>
                    <td>${t.금}</td>
                    <td>${t.은}</td>
                    <td>${t.동}</td>
                    <td>${t.총점}</td>
                `;
                teamTbody.appendChild(tr);
            });
        }
        
        // 참가자 삭제
        function deleteParticipant(id) {
            if (confirm('정말로 이 참가자를 삭제하시겠습니까?')) {
                participants = participants.filter(p => p.번호 !== id);
                scores = scores.filter(s => s.참가자번호 !== id);
                results = results.filter(r => r.번호 !== id);
                saveData();
                loadAllData();
                
                showNotification('참가자가 삭제되었습니다.', 'success');
            }
        }
        
        // 경기 기록 삭제
        function deleteScore(id) {
            if (confirm('정말로 이 경기 기록을 삭제하시겠습니까?')) {
                const score = scores.find(s => s.ID === id);
                scores = scores.filter(s => s.ID !== id);
                
                // 해당 수상 결과도 삭제
                if (score && score.수상 !== '-') {
                    results = results.filter(r => 
                        !(r.번호 === score.참가자번호 && 
                          r.경기종목 === score.경기종목 && 
                          r.수상 === score.수상)
                    );
                }
                
                saveData();
                loadScores();
                updateDashboard();
                updateRankings();
                
                showNotification('경기 기록이 삭제되었습니다.', 'success');
            }
        }
        
        // 시상 기준 삭제
        function deleteCriteria(event, grade) {
            if (confirm('정말로 이 시상 기준을 삭제하시겠습니까?')) {
                delete awardCriteria[event][grade];
                
                // 해당 종목에 기준이 더 이상 없으면 전체 종목도 삭제
                if (Object.keys(awardCriteria[event]).length === 0) {
                    delete awardCriteria[event];
                }
                
                saveData();
                loadCriteria();
                
                showNotification('시상 기준이 삭제되었습니다.', 'success');
            }
        }
        
        // 필터 변경 시 업데이트
        document.getElementById('scoreFilterEvent').addEventListener('change', loadScores);
        document.getElementById('criteriaFilterEvent').addEventListener('change', loadCriteria);
        document.getElementById('rankingFilter').addEventListener('change', updateRankings);
        document.getElementById('participantFilterTeam').addEventListener('input', loadParticipants);
        document.getElementById('participantFilterGrade').addEventListener('change', loadParticipants);
        
        // 초기화
        initializeData();
    </script>
</body>
</html>
