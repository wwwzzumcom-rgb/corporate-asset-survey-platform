import React, { useState, useEffect, useMemo, useRef } from 'react';
import { 
  Search, Download, Upload, Plus, AlertTriangle, CheckCircle, 
  LogOut, Edit2, Database, FileText, Check, X, RefreshCw, 
  Layers, UserCheck, Filter, AlertCircle, FileSpreadsheet, Lock, ShieldCheck, CheckSquare, Save
} from 'lucide-react';
import { initializeApp } from 'firebase/app';
import { 
  getAuth, 
  signInWithCustomToken, 
  signInAnonymously, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  getFirestore, 
  collection, 
  doc, 
  setDoc, 
  getDoc, 
  onSnapshot, 
  updateDoc, 
  deleteDoc 
} from 'firebase/firestore';

// ----------------------------------------------------------------------
// 1. Firebase 및 환경 설정 (시스템 실시간 연동)
// ----------------------------------------------------------------------
const firebaseConfig = typeof __firebase_config !== 'undefined' 
  ? JSON.parse(__firebase_config) 
  : {
      apiKey: "",
      authDomain: "mock-project.firebaseapp.com",
      projectId: "mock-project",
      storageBucket: "mock-project.appspot.com",
      messagingSenderId: "123456789",
      appId: "1:123456:web:123"
    };

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'it-asset-survey-v1';

// ----------------------------------------------------------------------
// 2. 기준 정보 정의 (조직도, 품목 구분 및 드롭다운 데이터)
// ----------------------------------------------------------------------
const DEPARTMENTS = [
  { id: "admin", upper: "관리자", team: "ADMIN", part: "관리자(ADMIN)" },
  { id: "dept1", upper: "경영지원실", team: "운영지원팀", part: "사업운영지원파트" },
  { id: "dept2", upper: "경영지원실", team: "운영지원팀", part: "HR지원파트" },
  { id: "dept3", upper: "경영지원실", team: "인사팀", part: "인사파트" },
  { id: "dept4", upper: "경영지원실", team: "인사팀", part: "인재개발파트" },
  { id: "dept5", upper: "GA영업부문", team: "GA영업1본부", part: "강북GA사업단-한강GA지점" },
  { id: "dept6", upper: "전속영업부문", team: "서울지역본부", part: "서울지역단-일산지점" }
];

const ITEMS = [
  { name: "pc본체(일반형)", code: "T" },
  { name: "pc본체(대리점용)", code: "H" },
  { name: "노트북", code: "N" },
  { name: "모니터", code: "M" },
  { name: "스캐너", code: "S" },
  { name: "프린터", code: "P" }
];

const PURPOSES = ["임직원", "대리점", "프로젝트", "공용", "인터넷pc", "교차", "sfp용"];

const SURVEY_STATUS = {
  "보유": ["QR등록", "QR미부착", "QR오류"],
  "미보유": ["반납", "망실", "손실", "이동"]
};

// 19자리 임의 자산번호 생성 함수
const generateAssetNo = () => {
  let result = '';
  for (let i = 0; i < 19; i++) {
    result += Math.floor(Math.random() * 10).toString();
  }
  return result;
};

// 파트별 10개씩 총 60개 자산 자동생성 제네레이터
const generateInitialAssets = () => {
  const result = [];
  const normalDepts = DEPARTMENTS.filter(d => d.id !== "admin");
  
  const lastNames = ["민준", "서연", "도윤", "서현", "예준", "수빈", "시우", "유진", "지호", "혜원"];
  const firstNames = ["김", "이", "박", "최", "정", "강", "조", "윤", "장", "임"];
  
  normalDepts.forEach((dept, deptIdx) => {
    for (let i = 1; i <= 10; i++) {
      const item = ITEMS[(i - 1) % ITEMS.length];
      
      let model = "";
      if (item.name === "pc본체(일반형)") model = "DB181-2(삼보)";
      else if (item.name === "pc본체(대리점용)") model = "50t(레노버)";
      else if (item.name === "모니터") model = "모니터(24인치)";
      else if (item.name === "노트북") model = "NT951XGK(삼성)";
      else if (item.name === "스캐너") model = "M160(캐논)";
      else if (item.name === "프린터") model = "HL-L6415DW(브라더)";

      const partSeed = String(deptIdx + 1);
      const rowSeed = String(i).padStart(2, "0");
      const randomPart = String(10000000 + i * 4927).padStart(8, "0");
      const assetNo = `3059481${partSeed}${rowSeed}${randomPart}1234`.substring(0, 19);

      const year = 2023 + (i % 4);
      const month = String((i % 12) + 1).padStart(2, '0');
      const day = String((i * 2 % 28) + 1).padStart(2, '0');
      const seqStr = String(i + deptIdx * 10).padStart(4, '0');
      const mgmtNo = `${item.code}${year}${month}${day}${seqStr}`;
      
      const userLastName = lastNames[(deptIdx + i) % lastNames.length];
      const userFirstName = firstNames[(deptIdx * 3 + i) % firstNames.length];
      const baseUser = `${userFirstName}${userLastName}`;

      const acquireDate = `${year}-${month}-${day}`;

      result.push({
        assetNo: assetNo,
        mgmtNo: mgmtNo,
        itemName: item.name,
        modelName: model,
        acquireDate: acquireDate,
        upperDept: dept.upper,
        dept: `${dept.team}-${dept.part}`,
        address: `${dept.team} 실내`,
        addressDetail: `${i}번 구역`,
        user: baseUser,
        surveyDept: "",
        surveyUser: "",
        purpose: "",
        status: "",
        statusDetail: "",
        basis: "",
        memo: "",
        submitStatus: "" // "" | "임시저장" | "제출완료"
      });
    }
  });
  return result;
};

export default function App() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [assets, setAssets] = useState([]);
  
  // 로그인 관련 (부서 드롭다운 + PW만 받음)
  const [selectedDeptId, setSelectedDeptId] = useState("dept1");
  const [userPasswordInput, setUserPasswordInput] = useState("");
  const [loginError, setLoginError] = useState("");
  const [currentUser, setCurrentUser] = useState(null);

  // 필터 및 검색 관련
  const [searchQuery, setSearchQuery] = useState("");
  const [filterUpperDept, setFilterUpperDept] = useState("전체");
  const [filterSurveyStatus, setFilterSurveyStatus] = useState("전체");
  const [filterItemName, setFilterItemName] = useState("전체");

  // 실시간 수기 인라인 편집 임시 보존 저장소
  const [rowEdits, setRowEdits] = useState({});

  // 상세 보기 및 기초자료 편집 모달 관련 (ADMIN 전용)
  const [isEditModalOpen, setIsEditModalOpen] = useState(false);
  const [selectedAsset, setSelectedAsset] = useState(null);
  const [isAddModalOpen, setIsAddModalOpen] = useState(false);

  // CSV 업로드 모달 관련
  const [isUploadModalOpen, setIsUploadModalOpen] = useState(false);
  const [csvText, setCsvText] = useState("");
  const [csvFeedback, setCsvFeedback] = useState(null);

  // 대시보드 상태 알림 메시지 모달
  const [notification, setNotification] = useState(null);

  // 샌드박스용 커스텀 모달 알림창 상태 관리
  const [customModal, setCustomModal] = useState(null); // null | { title, text, type: 'alert' | 'confirm', onConfirm }

  // 수기 추가용 Form State (ADMIN 전용, 고유 자산번호 직접입력 가능)
  const [formAsset, setFormAsset] = useState({
    assetNo: "",
    mgmtNo: "",
    itemName: "pc본체(일반형)",
    modelName: "DB181-2(삼보)",
    acquireDate: new Date().toISOString().substring(0, 10),
    upperDept: "경영지원실",
    dept: "운영지원팀-사업운영지원파트",
    address: "",
    addressDetail: "",
    user: "",
    surveyDept: "",
    surveyUser: "",
    purpose: "",
    status: "",
    statusDetail: "",
    basis: "",
    memo: "",
    submitStatus: ""
  });

  // ----------------------------------------------------------------------
  // 3. Firebase 인증 및 실시간 바인딩 (Rule 1, 2, 3 정합성 준수)
  // ----------------------------------------------------------------------
  useEffect(() => {
    const initAuth = async () => {
      try {
        setLoading(true);
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("인증 처리 실패:", err);
        showNotification("error", "서버 인증 환경 초기화에 실패했습니다.");
      } finally {
        setLoading(false);
      }
    };
    initAuth();

    const unsubscribeAuth = onAuthStateChanged(auth, (firebaseUser) => {
      setUser(firebaseUser);
    });

    return () => unsubscribeAuth();
  }, []);

  // Firestore 실시간 데이터 동기화
  useEffect(() => {
    if (!user) return;

    setLoading(true);
    const assetsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'assets');
    
    const unsubscribeAssets = onSnapshot(assetsCollection, (snapshot) => {
      const assetList = [];
      snapshot.forEach((doc) => {
        assetList.push({ id: doc.id, ...doc.data() });
      });
      
      if (assetList.length === 0) {
        setupInitialDatabase();
      } else {
        setAssets(assetList);
        setLoading(false);
      }
    }, (error) => {
      console.error("Firestore 수신 오류:", error);
      showNotification("error", `실시간 동기화 에러: ${error.message}`);
      setLoading(false);
    });

    return () => unsubscribeAssets();
  }, [user]);

  // DB 60대 자동 구축 및 동기화 함수
  const setupInitialDatabase = async () => {
    try {
      setLoading(true);
      const assetsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'assets');
      const seedList = generateInitialAssets();
      
      for (const asset of seedList) {
        const docRef = doc(assetsCollection, asset.assetNo);
        await setDoc(docRef, asset);
      }
      showNotification("success", "초기 자산조사용 60대 기초 DB 구축이 완료되었습니다.");
      setLoading(false);
    } catch (err) {
      console.error("초기 자산 셋업 실패:", err);
      showNotification("error", "초기 DB 자산 구동 중 문제가 발생했습니다.");
      setLoading(false);
    }
  };

  // 알림 도우미
  const showNotification = (type, text) => {
    setNotification({ type, text });
    setTimeout(() => {
      setNotification(null);
    }, 4000);
  };

  // 커스텀 모달 알림 전용 래퍼
  const showAlert = (text) => {
    setCustomModal({
      title: "알림 수신",
      text,
      type: "alert"
    });
  };

  const showConfirm = (text, onConfirm) => {
    setCustomModal({
      title: "동기화 의사 확인",
      text,
      type: "confirm",
      onConfirm
    });
  };

  // ----------------------------------------------------------------------
  // 4. 비즈니스 로직 및 유효성 검사 (자산번호 & 전산관리번호 정밀 검증)
  // ----------------------------------------------------------------------
  const getValidationErrors = (asset, isNew = false, allAssets = []) => {
    const errors = [];

    // 1. 자산번호 검증 (19자리 숫자 여부)
    if (!asset.assetNo || asset.assetNo.trim() === '') {
      errors.push("자산번호가 입력되지 않았습니다.");
    } else if (!/^\d{19}$/.test(asset.assetNo)) {
      errors.push("자산번호는 정확히 19자리의 숫자형태여야 합니다.");
    }

    // 2. 자산번호 중복 검증 (신규 생성 시에만 체크)
    if (isNew) {
      const isDuplicatedAssetNo = allAssets.some(item => item.assetNo === asset.assetNo);
      if (isDuplicatedAssetNo) {
        errors.push(`자산번호 [${asset.assetNo}]가 이미 전사에 존재합니다.`);
      }
    }

    // 3. 전산관리번호 규격 정밀 검증 (13자리: 품목구분알파벳 1자리 + 년월일 8자리 + 순번 4자리)
    const mgmtNo = asset.mgmtNo ? asset.mgmtNo.trim() : "";
    if (mgmtNo === '') {
      errors.push("전산관리번호가 누락되었습니다.");
    } else {
      if (!/^[THNMPS]\d{8}\d{4}$/.test(mgmtNo)) {
        errors.push("전산관리번호 규격 이상 (예: T202602180001 / 알파벳 1자리+년월일8자리+순번4자리 총 13자리)");
      } else {
        const matchingItem = ITEMS.find(i => i.name === asset.itemName);
        if (matchingItem && mgmtNo[0] !== matchingItem.code) {
          errors.push(`품목명(${asset.itemName}) 기호는 [${matchingItem.code}] 이나, 입력된 관리번호는 [${mgmtNo[0]}]로 시작하여 정합성이 깨졌습니다.`);
        }
      }
    }

    // 4. 전산관리번호 중복 검증
    if (isNew && mgmtNo !== '') {
      const isDuplicatedMgmtNo = allAssets.some(item => item.mgmtNo === mgmtNo);
      if (isDuplicatedMgmtNo) {
        errors.push(`전산관리번호 [${mgmtNo}]가 이미 전사 DB에 존재합니다.`);
      }
    }

    // 5. 실사 세부항목 의존 검증
    if (asset.status === "보유") {
      const allowed = SURVEY_STATUS["보유"];
      if (asset.statusDetail && !allowed.includes(asset.statusDetail)) {
        errors.push(`'보유' 상태의 세부 사항은 [${allowed.join(', ')}] 중 하나여야 합니다.`);
      }
    } else if (asset.status === "미보유") {
      const allowed = SURVEY_STATUS["미보유"];
      if (asset.statusDetail && !allowed.includes(asset.statusDetail)) {
        errors.push(`'미보유' 상태의 세부 사항은 [${allowed.join(', ')}] 중 하나여야 합니다.`);
      }
    }

    return errors;
  };

  // 실시간 모든 데이터 유효성 이상 진단 (품목명 불일치, 번호 오류 등)
  const validationAnomalies = useMemo(() => {
    const anomalies = [];
    assets.forEach(asset => {
      const errs = getValidationErrors(asset, false, []);
      
      const dupAssets = assets.filter(a => a.assetNo === asset.assetNo);
      if (dupAssets.length > 1) {
        errs.push("동일한 자산번호가 전사 DB 내에 2건 이상 중복 존재합니다.");
      }
      const dupMgmts = assets.filter(a => a.mgmtNo === asset.mgmtNo && asset.mgmtNo !== "");
      if (dupMgmts.length > 1) {
        errs.push("동일한 전산관리번호가 전사 DB 내에 2건 이상 중복 존재합니다.");
      }

      if (errs.length > 0) {
        anomalies.push({
          assetNo: asset.assetNo,
          mgmtNo: asset.mgmtNo || "미정",
          itemName: asset.itemName,
          dept: `${asset.upperDept} ${asset.dept}`,
          errors: errs
        });
      }
    });
    return anomalies;
  }, [assets]);

  // 현재 로그인한 부서의 결과제출 여부 판별 (제출상태가 하나라도 '제출완료'이면 제출된 것으로 잠금 처리)
  const isDeptSubmitted = useMemo(() => {
    if (!currentUser || currentUser.dept.id === 'admin') return false;
    const deptAssets = assets.filter(a => `${a.upperDept} ${a.dept}`.includes(currentUser.dept.part));
    if (deptAssets.length === 0) return false;
    return deptAssets.every(a => a.submitStatus === "제출완료");
  }, [assets, currentUser]);

  // 임시저장 내역 혹은 현재 미제출된 자산 중 작성한 내역이 존재하여 제출할 수 있는지 검사
  const canSubmit = useMemo(() => {
    if (isDeptSubmitted) return false;
    if (!currentUser || currentUser.dept.id === 'admin') return false;

    const deptAssets = assets.filter(a => `${a.upperDept} ${a.dept}`.includes(currentUser.dept.part));
    return deptAssets.some(asset => {
      const edits = rowEdits[asset.assetNo];
      const sDept = edits?.surveyDept !== undefined ? edits.surveyDept : (asset.surveyDept || "");
      const sUser = edits?.surveyUser !== undefined ? edits.surveyUser : (asset.surveyUser || "");
      const sPurp = edits?.purpose !== undefined ? edits.purpose : (asset.purpose || "");
      const sStat = edits?.status !== undefined ? edits.status : (asset.status || "");
      const sDetail = edits?.statusDetail !== undefined ? edits.statusDetail : (asset.statusDetail || "");
      
      return sDept || sUser || sPurp || sStat || sDetail || asset.submitStatus === "임시저장";
    });
  }, [assets, rowEdits, currentUser, isDeptSubmitted]);

  // ----------------------------------------------------------------------
  // 5. 로그인 및 로그아웃 정의
  // ----------------------------------------------------------------------
  const handleLogin = (e) => {
    e.preventDefault();
    setLoginError("");

    const targetDept = DEPARTMENTS.find(d => d.id === selectedDeptId);
    if (!targetDept) {
      setLoginError("해당 부서를 찾을 수 없습니다.");
      return;
    }

    // 비밀번호 정합성 검증 시뮬레이션
    const isCorrect = (targetDept.id === 'admin' && userPasswordInput === 'admin') ||
                      (targetDept.id !== 'admin' && userPasswordInput === '1234');

    if (isCorrect) {
      setCurrentUser({
        name: targetDept.id === 'admin' ? "관리자" : `${targetDept.team} 담당자`,
        dept: targetDept
      });
      setUserPasswordInput("");
      showNotification("success", `${targetDept.part} 권한으로 안전하게 연결되었습니다.`);
    } else {
      setLoginError("입력하신 비밀번호가 정합하지 않습니다. 다시 시도하십시오.");
    }
  };

  const handleLogout = () => {
    setCurrentUser(null);
    setRowEdits({});
    showNotification("success", "성공적으로 로그아웃되었습니다.");
  };

  // ----------------------------------------------------------------------
  // 6. 데이터 조작 기능 (일괄 저장/제출 제어 탑재)
  // ----------------------------------------------------------------------
  
  // 자산 수기 추가 (ADMIN 전용) - 자산 고유번호 입력 필드 개방
  const handleCreateAsset = async (e) => {
    e.preventDefault();
    if (currentUser?.dept?.id !== 'admin') {
      showAlert("권한이 없습니다. 관리자만 이용할 수 있습니다.");
      return;
    }
    
    const errs = getValidationErrors(formAsset, true, assets);
    if (errs.length > 0) {
      showAlert(`데이터 규칙 위반 또는 중복이 존재합니다.\n\n${errs.join('\n')}`);
      return;
    }

    try {
      const docRef = doc(collection(db, 'artifacts', appId, 'public', 'data', 'assets'), formAsset.assetNo);
      await setDoc(docRef, {
        ...formAsset,
        updatedBy: `관리자 ${currentUser.name}`,
        updatedAt: new Date().toISOString()
      });

      setIsAddModalOpen(false);
      showNotification("success", `원천 DB에 신규 자산이 성공적으로 등록되었습니다.`);
    } catch (err) {
      console.error("추가 에러:", err);
      showNotification("error", "자산 저장 중 에러가 발생했습니다.");
    }
  };

  // 인라인 실사 폼 입력 핸들러
  const handleFieldChange = (assetNo, field, value) => {
    if (isDeptSubmitted) return; // 제출 완료 상태 시 인라인 편집도 무조건 차단

    setRowEdits(prev => {
      const currentAsset = assets.find(a => a.assetNo === assetNo) || {};
      const currentEdit = prev[assetNo] || {
        surveyDept: currentAsset.surveyDept || "",
        surveyUser: currentAsset.surveyUser || "",
        purpose: currentAsset.purpose || "",
        status: currentAsset.status || "",
        statusDetail: currentAsset.statusDetail || "",
        basis: currentAsset.basis || "",
        memo: currentAsset.memo || ""
      };

      const updated = { ...currentEdit, [field]: value };

      if (field === "status") {
        if (value === "보유") {
          updated.statusDetail = "QR등록";
        } else if (value === "미보유") {
          updated.statusDetail = "반납";
        } else {
          updated.statusDetail = "";
        }
      }

      return { ...prev, [assetNo]: updated };
    });
  };

  // 내 소속 일괄 입력 제어 함수 (상단 통합 단일 버튼)
  const handleApplyMyDeptAll = () => {
    if (isDeptSubmitted) return; 
    if (!currentUser || currentUser.dept.id === 'admin') {
      showAlert("부서 담당자 권한으로 로그인 한 상태에서만 자동입력이 지원됩니다.");
      return;
    }

    const fullDeptString = `${currentUser.dept.upper}-${currentUser.dept.team}-${currentUser.dept.part}`;
    
    filteredAssets.forEach(asset => {
      handleFieldChange(asset.assetNo, 'surveyDept', fullDeptString);
    });

    showNotification("success", "화면상의 모든 대상에 '실제 현 사용부서'가 현재 로그인 부서 정보로 일괄 입력되었습니다. (저장을 완료하려면 아래 임시저장을 클릭해 주세요)");
  };

  // 일괄 임시저장
  const handleBatchSaveTemp = async () => {
    if (isDeptSubmitted) return; 

    const targetsToSave = [];
    let validationFailed = false;
    let failedMsg = "";

    filteredAssets.forEach(asset => {
      const edits = rowEdits[asset.assetNo];
      if (edits || asset.submitStatus !== "제출완료") {
        const merged = {
          assetNo: asset.assetNo,
          mgmtNo: asset.mgmtNo,
          itemName: asset.itemName,
          surveyDept: edits?.surveyDept !== undefined ? edits.surveyDept : (asset.surveyDept || ""),
          surveyUser: edits?.surveyUser !== undefined ? edits.surveyUser : (asset.surveyUser || ""),
          purpose: edits?.purpose !== undefined ? edits.purpose : (asset.purpose || ""),
          status: edits?.status !== undefined ? edits.status : (asset.status || ""),
          statusDetail: edits?.statusDetail !== undefined ? edits.statusDetail : (asset.statusDetail || ""),
          basis: edits?.basis !== undefined ? edits.basis : (asset.basis || ""),
          memo: edits?.memo !== undefined ? edits.memo : (asset.memo || "")
        };

        const errs = getValidationErrors(merged, false, assets);
        if (errs.length > 0) {
          validationFailed = true;
          failedMsg += `• 자산 [${merged.assetNo} / ${merged.mgmtNo || '번호누락'}]:\n  ${errs.join('\n  ')}\n`;
        } else {
          targetsToSave.push(merged);
        }
      }
    });

    if (validationFailed) {
      showAlert(`임시저장 불가: 아래 항목의 규격 정합성을 확인 및 수정해 주세요.\n\n${failedMsg}`);
      return;
    }

    if (targetsToSave.length === 0) {
      showNotification("success", "수정 사항이 없거나 이미 임시저장이 완료된 상태입니다.");
      return;
    }

    try {
      setLoading(true);
      for (const t of targetsToSave) {
        const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', t.assetNo);
        await updateDoc(docRef, {
          surveyDept: t.surveyDept,
          surveyUser: t.surveyUser,
          purpose: t.purpose,
          status: t.status,
          statusDetail: t.statusDetail,
          basis: t.basis,
          memo: t.memo,
          submitStatus: "임시저장",
          updatedBy: `${currentUser.dept.part} ${currentUser.name}`,
          updatedAt: new Date().toISOString()
        });
      }

      setRowEdits({});
      showNotification("success", `총 ${targetsToSave.length}건의 자산 실사결과가 '임시저장' 상태로 실시간 반영되었습니다.`);
      setLoading(false);
    } catch (err) {
      console.error("일괄 임시저장 실패:", err);
      showNotification("error", "임시저장 처리 중 에러가 발생했습니다.");
      setLoading(false);
    }
  };

  // 일괄 결과 제출 (ADMIN이 수정한 사항 및 사용자가 입력한 내용을 안전하게 보존하며 일괄 제출 및 잠금)
  const handleBatchSubmitFinal = () => {
    if (isDeptSubmitted) return;
    if (!currentUser || currentUser.dept.id === 'admin') return;

    const deptAssets = assets.filter(a => `${a.upperDept} ${a.dept}`.includes(currentUser.dept.part));
    let checkFailed = false;
    let failedDetails = "";
    const targetsToSave = [];

    deptAssets.forEach(asset => {
      const edits = rowEdits[asset.assetNo] || {};
      const surveyDept = edits.surveyDept !== undefined ? edits.surveyDept : (asset.surveyDept || "");
      const surveyUser = edits.surveyUser !== undefined ? edits.surveyUser : (asset.surveyUser || "");
      const purpose = edits.purpose !== undefined ? edits.purpose : (asset.purpose || "");
      const status = edits.status !== undefined ? edits.status : (asset.status || "");
      const statusDetail = edits.statusDetail !== undefined ? edits.statusDetail : (asset.statusDetail || "");
      const basis = edits.basis !== undefined ? edits.basis : (asset.basis || "");
      const memo = edits.memo !== undefined ? edits.memo : (asset.memo || "");

      // 요구사항 2번: 필수항목 기입 엄격 검증
      if (!surveyDept.trim() || !surveyUser.trim() || !purpose || !status || !statusDetail) {
        checkFailed = true;
        failedDetails += `• 자산번호 ${asset.assetNo} (${asset.modelName || '기기'}): 실제 부서, 실제 사용자, 용도, 상태, 상태세부 필수 기입 누락\n`;
      } else {
        // ADMIN이 수동 등록 또는 수정 창에서 수정한 기본 속성은 건드리지 않고 실사 데이터와 제출상태만 업데이트하도록 전송 페이로드 구성
        targetsToSave.push({
          assetNo: asset.assetNo,
          payload: {
            surveyDept,
            surveyUser,
            purpose,
            status,
            statusDetail,
            basis,
            memo,
            submitStatus: "제출완료",
            updatedBy: `${currentUser.dept.part} ${currentUser.name} 최종제출`,
            updatedAt: new Date().toISOString()
          }
        });
      }
    });

    if (checkFailed) {
      showAlert(`결과제출 실패: 조사 대상 중 아래 누락된 필수항목이 존재합니다.\n\n${failedDetails}`);
      return;
    }

    showConfirm(`소속 자산 ${deptAssets.length}건에 대한 실사결과를 최종 제출하시겠습니까? 제출 후에는 수정 필드 및 제어 버튼들이 모두 비활성화 잠금 상태로 전환됩니다.`, async () => {
      try {
        setLoading(true);
        for (const target of targetsToSave) {
          const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', target.assetNo);
          await updateDoc(docRef, target.payload);
        }

        setRowEdits({}); // 임시 로컬 수정본 클리어
        showNotification("success", `총 ${targetsToSave.length}건의 자산 실사가 성공적으로 제출 완료 및 비활성화 처리되었습니다.`);
        setLoading(false);
      } catch (err) {
        console.error("일괄 제출 오류:", err);
        showNotification("error", "최종 제출 전송 중 오류가 발생했습니다.");
        setLoading(false);
      }
    });
  };

  // '제출취소' 버튼 클릭 핸들러 (부서의 모든 기기 제출완료 상태를 해제하여 다시 활성화시킴)
  const handleBatchCancelSubmit = () => {
    if (!currentUser || currentUser.dept.id === 'admin') return;

    showConfirm("제출을 공식적으로 취소하고 다시 입력 및 수정을 활성화하시겠습니까?", async () => {
      try {
        setLoading(true);
        const deptAssets = assets.filter(a => `${a.upperDept} ${a.dept}`.includes(currentUser.dept.part));
        for (const asset of deptAssets) {
          const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', asset.assetNo);
          await updateDoc(docRef, {
            submitStatus: "임시저장", 
            updatedBy: `${currentUser.dept.part} ${currentUser.name} 제출취소`,
            updatedAt: new Date().toISOString()
          });
        }
        showNotification("success", "제출이 취소되었습니다. 이제 모든 입력 및 수정 작업이 다시 활성화되었습니다.");
        setLoading(false);
      } catch (err) {
        console.error("제출 취소 중 서버 에러:", err);
        showNotification("error", "제출 취소 처리 중 오류가 발생했습니다.");
        setLoading(false);
      }
    });
  };

  // 원천 기본정보 강제 수정 (ADMIN 전용 모달 동작)
  const handleUpdateAssetBase = async (e) => {
    e.preventDefault();
    if (currentUser?.dept?.id !== 'admin') {
      showAlert("권한이 없습니다.");
      return;
    }

    const errs = getValidationErrors(selectedAsset, false, assets);
    if (errs.length > 0) {
      showAlert(`저장 불가: 아래 오기입 사항을 수정해주세요.\n\n${errs.join('\n')}`);
      return;
    }

    try {
      setLoading(true);
      const assetsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'assets');
      
      if (selectedAsset.id && selectedAsset.assetNo !== selectedAsset.id) {
        const oldDocRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', selectedAsset.id);
        await deleteDoc(oldDocRef);
      }

      const newDocRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', selectedAsset.assetNo);
      const updatedData = { ...selectedAsset };
      delete updatedData.id; 

      await setDoc(newDocRef, {
        ...updatedData,
        updatedBy: `관리자 수정`,
        updatedAt: new Date().toISOString()
      });

      setIsEditModalOpen(false);
      showNotification("success", `관리자에 의해 원천 정보가 성공적으로 반영되었습니다.`);
      setLoading(false);
    } catch (err) {
      console.error("수정 에러:", err);
      showNotification("error", "업데이트 중 오류가 발생했습니다.");
      setLoading(false);
    }
  };

  // 자산 삭제 기능 (ADMIN 전용)
  const handleDeleteAsset = (assetNo) => {
    if (currentUser?.dept?.id !== 'admin') {
      showAlert("권한이 없습니다.");
      return;
    }
    showConfirm("정말로 이 자산을 원천 데이터베이스에서 영구 삭제하시겠습니까?", async () => {
      try {
        const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'assets', assetNo);
        await deleteDoc(docRef);
        setIsEditModalOpen(false);
        showNotification("success", "지정한 자산 원천정보가 실시간 영구 삭제되었습니다.");
      } catch (err) {
        console.error("삭제 에러:", err);
      }
    });
  };

  // ----------------------------------------------------------------------
  // 7. CSV 파싱 및 내보내기 (일반 부서 및 ADMIN 공용 실사결과 연동 지원)
  // ----------------------------------------------------------------------
  
  // 전체 결과 CSV 다운로드 (ADMIN 계정일 경우, 필터에 상관없이 모든 부서 실사 취합 본 전체 리스트 출력)
  const handleExportCSV = () => {
    const isUserAdmin = currentUser?.dept?.id === 'admin';
    const assetsToExport = isUserAdmin ? assets : filteredAssets;

    const headers = [
      "자산번호", "전산관리번호", "품목명", "모델명", "취득연월일", "상위부서", "사용부서", "주소", "상세주소", "사용자", 
      "실제현사용부서", "실제현사용자", "용도", "상태", "상태세부내용", "관련근거", "비고", "제출상태"
    ];

    const rows = assetsToExport.map(a => [
      a.assetNo || "",
      a.mgmtNo || "",
      a.itemName || "",
      a.modelName || "",
      a.acquireDate || "",
      a.upperDept || "",
      a.dept || "",
      a.address || "",
      a.addressDetail || "",
      a.user || "",
      a.surveyDept || "",
      a.surveyUser || "",
      a.purpose || "",
      a.status || "",
      a.statusDetail || "",
      a.basis || "",
      a.memo || "",
      a.submitStatus || ""
    ]);

    const csvContent = [
      headers.join(","),
      ...rows.map(row => row.map(val => `"${val.replace(/"/g, '""')}"`).join(","))
    ].join("\n");

    const blob = new Blob(["\uFEFF" + csvContent], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement("a");
    link.setAttribute("href", url);
    link.setAttribute("download", isUserAdmin ? `전사_전산기기_자산실사결과_취합대장.csv` : `전산기기_자산실사결과_${new Date().toISOString().substring(0, 10)}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    showNotification("success", isUserAdmin ? "전사 취합 자산 실사 결과가 CSV로 다운로드되었습니다." : "소속 자산 실사 내역이 CSV로 내보내졌습니다.");
  };

  // CSV 가져오기 분석 및 실시간 반영
  const handleUploadCSV = () => {
    if (isDeptSubmitted) {
      showAlert("이미 결과제출이 완료된 부서입니다. 실사 업로드를 진행하려면 먼저 제출취소를 해주세요.");
      return;
    }

    if (!csvText.trim()) {
      showAlert("업로드할 CSV 내용을 입력해 주세요.");
      return;
    }

    const lines = csvText.split('\n').map(line => line.trim()).filter(line => line.length > 0);
    if (lines.length <= 1) {
      showAlert("헤더 외에 등록할 데이터 레코드가 없습니다.");
      return;
    }

    const header = lines[0].split(',').map(h => h.replace(/^"|"$/g, '').trim());
    const dataLines = lines.slice(1);

    const parsedAssets = [];
    let errorsFound = 0;
    const feedbackList = [];

    const getIndex = (possibleNames) => {
      return header.findIndex(h => possibleNames.some(pn => h.includes(pn)));
    };

    const idxAssetNo = getIndex(["자산번호", "assetNo"]);
    const idxMgmtNo = getIndex(["전산관리번호", "mgmtNo", "관리번호"]);
    const idxItemName = getIndex(["품목명", "itemName", "품목"]);
    const idxModelName = getIndex(["모델명", "modelName", "모델"]);
    const idxAcquireDate = getIndex(["취득연월일", "acquireDate", "취득"]);
    const idxUpperDept = getIndex(["상위부서", "upperDept"]);
    const idxDept = getIndex(["사용부서", "dept"]);
    const idxAddress = getIndex(["주소", "address"]);
    const idxAddressDetail = getIndex(["상세주소", "addressDetail"]);
    const idxUser = getIndex(["사용자", "user"]);
    const idxSurveyDept = getIndex(["실제현사용부서", "실제 현 사용부서", "surveyDept"]);
    const idxSurveyUser = getIndex(["실제현사용자", "실제 현 사용자", "surveyUser", "실제현사용자"]);
    const idxPurpose = getIndex(["용도", "purpose"]);
    const idxStatus = getIndex(["상태", "status"]);
    const idxStatusDetail = getIndex(["상태세부내용", "상태 세부내용", "statusDetail"]);
    const idxBasis = getIndex(["관련근거", "basis"]);
    const idxMemo = getIndex(["비고", "memo"]);

    dataLines.forEach((line, index) => {
      const matches = line.match(/(".*?"|[^",\s]+)(?=\s*,|\s*$)/g) || line.split(',');
      const cleanParts = matches.map(part => part.replace(/^"|"$/g, '').trim());

      const getVal = (idx, fallback = "") => (idx !== -1 && idx < cleanParts.length) ? cleanParts[idx] : fallback;

      let parsedAssetNo = getVal(idxAssetNo);
      if (!parsedAssetNo) {
        parsedAssetNo = generateAssetNo();
      }

      const rowAsset = {
        assetNo: parsedAssetNo,
        mgmtNo: getVal(idxMgmtNo),
        itemName: getVal(idxItemName, "pc본체(일반형)"),
        modelName: getVal(idxModelName, "미정"),
        acquireDate: getVal(idxAcquireDate, new Date().toISOString().substring(0, 10)),
        upperDept: getVal(idxUpperDept, "경영지원실"),
        dept: getVal(idxDept, "운영지원팀-사업운영지원파트"),
        address: getVal(idxAddress, ""),
        addressDetail: getVal(idxAddressDetail, ""),
        user: getVal(idxUser, "미정"),
        surveyDept: getVal(idxSurveyDept, ""),
        surveyUser: getVal(idxSurveyUser, ""),
        purpose: getVal(idxPurpose, ""),
        status: getVal(idxStatus, ""),
        statusDetail: getVal(idxStatusDetail, ""),
        basis: getVal(idxBasis, ""),
        memo: getVal(idxMemo, ""),
        submitStatus: "임시저장"
      };

      if (currentUser && currentUser.dept.id !== 'admin') {
        const userPart = currentUser.dept.part;
        const assetFullDept = `${rowAsset.upperDept} ${rowAsset.dept}`;
        if (!assetFullDept.includes(userPart)) {
          errorsFound++;
          feedbackList.push({
            row: index + 2,
            mgmtNo: rowAsset.mgmtNo,
            errors: ["타부서 자산에 대한 일괄 실사 업로드 권한이 없습니다."],
            critical: true
          });
          return;
        }
      }

      const rowErrors = getValidationErrors(rowAsset, false, []);
      if (rowErrors.length > 0) {
        errorsFound++;
        feedbackList.push({
          row: index + 2,
          mgmtNo: rowAsset.mgmtNo,
          errors: rowErrors,
          critical: true
        });
      } else {
        parsedAssets.push(rowAsset);
        feedbackList.push({
          row: index + 2,
          mgmtNo: rowAsset.mgmtNo,
          errors: ["정합성 통과 (임시저장 대기)"],
          critical: false
        });
      }
    });

    setCsvFeedback({
      total: dataLines.length,
      successCount: parsedAssets.length,
      errorCount: errorsFound,
      details: feedbackList
    });

    if (parsedAssets.length > 0) {
      showConfirm(`검증이 통과된 ${parsedAssets.length}건의 자산에 대해 실사 결과를 일괄 임시저장 등록 처리할까요?`, async () => {
        try {
          const assetsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'assets');
          for (const item of parsedAssets) {
            const docRef = doc(assetsCollection, item.assetNo);
            await setDoc(docRef, {
              ...item,
              updatedBy: `${currentUser.dept.part} 일괄엑셀실사`,
              updatedAt: new Date().toISOString()
            });
          }
          showNotification("success", `${parsedAssets.length}건의 자산 실사 결과가 일괄 임시저장으로 정상 갱신되었습니다.`);
          setIsUploadModalOpen(false);
          setCsvText("");
          setCsvFeedback(null);
        } catch (err) {
          console.error("일괄 실사 업로드 에러:", err);
          showAlert("저장 실패: " + err.message);
        }
      });
    } else {
      showAlert("규격을 만족하거나 권한에 부합하는 자산 레코드가 없습니다. 오류 내역을 보완해 주세요.");
    }
  };

  // ----------------------------------------------------------------------
  // 8. 통계 처리 및 데이터 권한 필터링 (요구사항 1번 및 2번 완벽 반영)
  // ----------------------------------------------------------------------
  const stats = useMemo(() => {
    const myDeptAssets = assets.filter(asset => {
      if (currentUser && currentUser.dept.id !== 'admin') {
        const userPart = currentUser.dept.part;
        const assetFullDept = `${asset.upperDept} ${asset.dept}`;
        return assetFullDept.includes(userPart);
      }
      return true; // admin은 전체
    });

    const total = myDeptAssets.length;

    // 기재된 5개 항목이 모두 기입 완료된 것만 '조사 완료'로 엄격 반영
    const completed = myDeptAssets.filter(a => {
      const sDept = a.surveyDept ? a.surveyDept.trim() : "";
      const sUser = a.surveyUser ? a.surveyUser.trim() : "";
      const sPurp = a.purpose;
      const sStat = a.status;
      const sDetail = a.statusDetail;
      return sDept && sUser && sPurp && sStat && sDetail;
    }).length;

    const rate = total > 0 ? Math.round((completed / total) * 100) : 0;
    const anomalyCount = validationAnomalies.length;

    // 부서별 지표 가공 (공식제출 결과제출 완료 여부 상태 반영)
    const deptProgress = {};
    DEPARTMENTS.filter(d => d.id !== "admin").forEach(d => {
      const deptAssets = assets.filter(a => `${a.upperDept} ${a.dept}`.includes(d.part));
      const deptTotal = deptAssets.length;
      
      const deptCompleted = deptAssets.filter(a => {
        const sDept = a.surveyDept ? a.surveyDept.trim() : "";
        const sUser = a.surveyUser ? a.surveyUser.trim() : "";
        const sPurp = a.purpose;
        const sStat = a.status;
        const sDetail = a.statusDetail;
        return sDept && sUser && sPurp && sStat && sDetail;
      }).length;
      
      const deptRate = deptTotal > 0 ? Math.round((deptCompleted / deptTotal) * 100) : 0;
      const isOfficialSubmitted = deptTotal > 0 && deptAssets.every(a => a.submitStatus === "제출완료");

      deptProgress[d.id] = {
        name: d.part,
        total: deptTotal,
        completed: deptCompleted,
        rate: deptRate,
        submitted: isOfficialSubmitted 
      };
    });

    return { total, completed, rate, anomalyCount, deptProgress };
  }, [assets, validationAnomalies, currentUser]);

  // 최종 리스트 표출 목록 (필터 및 권한 반영)
  const filteredAssets = useMemo(() => {
    return assets.filter(asset => {
      if (currentUser && currentUser.dept.id !== 'admin') {
        const userPart = currentUser.dept.part;
        const assetFullDept = `${asset.upperDept} ${asset.dept}`;
        if (!assetFullDept.includes(userPart)) {
          return false;
        }
      }

      const q = searchQuery.toLowerCase();
      const matchSearch = q === "" || 
        (asset.assetNo && asset.assetNo.includes(q)) ||
        (asset.mgmtNo && asset.mgmtNo.toLowerCase().includes(q)) ||
        (asset.modelName && asset.modelName.toLowerCase().includes(q)) ||
        (asset.user && asset.user.toLowerCase().includes(q)) ||
        (asset.surveyUser && asset.surveyUser.toLowerCase().includes(q));

      const matchUpperDept = filterUpperDept === "전체" || asset.upperDept === filterUpperDept;
      const matchItemName = filterItemName === "전체" || asset.itemName === filterItemName;

      let matchSurvey = true;
      if (filterSurveyStatus === "완료") {
        matchSurvey = asset.submitStatus === "제출완료" || asset.submitStatus === "임시저장";
      } else if (filterSurveyStatus === "미조사") {
        matchSurvey = asset.submitStatus !== "제출완료" && asset.submitStatus !== "임시저장";
      }

      return matchSearch && matchUpperDept && matchItemName && matchSurvey;
    });
  }, [assets, searchQuery, filterUpperDept, filterItemName, filterSurveyStatus, currentUser]);

  // 상단 일괄 제출버튼 활성화 여부 모니터링 (submitStatus === '임시저장'인 항목 유무)
  const hasTempSaved = useMemo(() => {
    return filteredAssets.some(a => a.submitStatus === "임시저장");
  }, [filteredAssets]);


  // ----------------------------------------------------------------------
  // 9. 렌더링 UI 영역 (HTML/React CSS 레이아웃)
  // ----------------------------------------------------------------------
  return (
    <div className="min-h-screen bg-slate-900 text-slate-100 font-sans flex flex-col antialiased selection:bg-indigo-500 selection:text-white">
      
      {/* 실시간 알림 피드백 토스트 */}
      {notification && (
        <div className="fixed top-5 right-5 z-50 animate-bounce">
          <div className={`flex items-center gap-2 px-4 py-3 rounded-xl shadow-2xl border text-xs font-bold ${
            notification.type === "success" 
              ? "bg-emerald-950/90 border-emerald-500/30 text-emerald-300" 
              : "bg-rose-950/90 border-rose-500/30 text-rose-300"
          }`}>
            {notification.type === "success" ? <CheckCircle className="w-4 h-4" /> : <AlertTriangle className="w-4 h-4" />}
            {notification.text}
          </div>
        </div>
      )}

      {/* 로딩 인디케이터 백드롭 */}
      {loading && (
        <div className="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex flex-col items-center justify-center gap-3">
          <RefreshCw className="w-10 h-10 text-indigo-500 animate-spin" />
          <p className="text-xs text-indigo-300 font-bold tracking-wider">실시간 데이터베이스 동기화 중...</p>
        </div>
      )}

      {/* ----------------------------------------------------------------------
          A. 최초 로그인 화면 (부서 선택 + 비밀번호, 줄바꿈 방지 적용)
          ---------------------------------------------------------------------- */}
      {!currentUser ? (
        <div className="flex-1 flex items-center justify-center px-4 py-12 relative overflow-hidden bg-[radial-gradient(ellipse_at_top,_var(--tw-gradient-stops))] from-slate-900 via-slate-950 to-black">
          <div className="absolute inset-0 bg-grid-slate-800/[0.05] bg-[size:30px_30px]" />
          
          <div className="max-w-md w-full space-y-8 bg-slate-800/60 p-8 rounded-2xl border border-slate-700/50 backdrop-blur-xl shadow-2xl relative z-10">
            <div className="text-center">
              <div className="inline-flex p-3 bg-indigo-500/10 rounded-2xl border border-indigo-500/30 text-indigo-400 mb-4">
                <Database className="w-10 h-10" />
              </div>
              <h2 className="text-2xl sm:text-3xl font-extrabold tracking-tight bg-gradient-to-r from-white via-indigo-200 to-indigo-400 bg-clip-text text-transparent whitespace-nowrap">
                전사 전산기기 자산조사
              </h2>
            </div>

            <form className="mt-8 space-y-6" onSubmit={handleLogin}>
              {loginError && (
                <div className="p-3 bg-rose-500/10 border border-rose-500/20 rounded-lg text-xs text-rose-400 flex items-center gap-2">
                  <AlertCircle className="w-4 h-4" /> {loginError}
                </div>
              )}

              <div className="space-y-4">
                <div>
                  <label className="block text-xs font-semibold text-slate-400 uppercase tracking-wider mb-2">
                    부서 선택
                  </label>
                  <select
                    value={selectedDeptId}
                    onChange={(e) => {
                      setSelectedDeptId(e.target.value);
                      setLoginError("");
                    }}
                    className="w-full bg-slate-900/90 border border-slate-700 rounded-xl px-4 py-3 text-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition"
                  >
                    {DEPARTMENTS.map(d => (
                      <option key={d.id} value={d.id}>
                        {d.id === 'admin' ? `★ ${d.part}` : `[${d.upper}] ${d.team} - ${d.part}`}
                      </option>
                    ))}
                  </select>
                </div>

                <div>
                  <label className="block text-xs font-semibold text-slate-400 uppercase tracking-wider mb-2">
                    비밀번호 입력
                  </label>
                  <input
                    type="password"
                    required
                    value={userPasswordInput}
                    onChange={(e) => setUserPasswordInput(e.target.value)}
                    placeholder="패스워드를 기입하세요."
                    className="w-full bg-slate-900/90 border border-slate-700 rounded-xl px-4 py-3 text-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition"
                  />
                  <p className="text-[11px] text-slate-500 mt-2">
                    ※ 시뮬레이션: <span className="text-amber-400 font-semibold">부서 비번: 1234</span> / <span className="text-indigo-400 font-semibold">관리자 비번: admin</span>
                  </p>
                </div>
              </div>

              <div>
                <button
                  type="submit"
                  className="w-full flex justify-center py-3 px-4 border border-transparent text-sm font-semibold rounded-xl text-white bg-gradient-to-r from-indigo-600 to-violet-600 hover:from-indigo-500 hover:to-violet-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 shadow-lg shadow-indigo-500/20 transition-all transform hover:-translate-y-0.5 active:translate-y-0"
                >
                  로그인
                </button>
              </div>
            </form>

            <div className="pt-4 border-t border-slate-700/40 text-center">
              <span className="text-xs text-slate-500">
                실시간 채널 ID: <code className="text-indigo-400">{appId}</code>
              </span>
            </div>
          </div>
        </div>
      ) : (
        
        // ----------------------------------------------------------------------
        // B. 메인 플랫폼 업무 영역 (인라인 실사 테이블 + 일괄 제어 포함)
        // ----------------------------------------------------------------------
        <div className="flex-1 flex flex-col">
          
          {/* 상단 네비게이션 헤더 */}
          <header className="bg-slate-950/80 backdrop-blur-md border-b border-slate-800/70 sticky top-0 z-40 px-6 py-4 flex flex-col md:flex-row items-center justify-between gap-4">
            <div className="flex items-center gap-3">
              <div className="p-2 bg-indigo-500/10 text-indigo-400 rounded-xl border border-indigo-500/20">
                <Layers className="w-6 h-6" />
              </div>
              <div>
                <h1 className="text-xl font-black text-white tracking-wide flex items-center gap-2">
                  전사 전산기기 자산조사
                  <span className="text-xs px-2.5 py-0.5 rounded-full bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 font-mono">
                    Realtime System
                  </span>
                </h1>
              </div>
            </div>

            {/* 로그인 소속 및 세션 */}
            <div className="flex items-center gap-4 bg-slate-900 px-4 py-2 rounded-xl border border-slate-800">
              <div className="text-right">
                <span className="text-xs text-slate-400 block">현재 소속 권한</span>
                <span className="text-sm font-semibold text-indigo-300">
                  {currentUser.dept.id === 'admin' ? "★ 최고 관리자 권한" : `[${currentUser.dept.upper}] ${currentUser.dept.part}`}
                </span>
              </div>
              <div className="h-8 w-[1px] bg-slate-800" />
              <div className="flex items-center gap-2">
                <div className="bg-indigo-600/20 text-indigo-300 p-1.5 rounded-lg text-xs font-bold flex items-center gap-1">
                  {currentUser.dept.id === 'admin' ? <ShieldCheck className="w-3.5 h-3.5 text-indigo-400" /> : <UserCheck className="w-3.5 h-3.5 text-slate-400" />}
                  {currentUser.name}
                </div>
              </div>
              <button
                onClick={handleLogout}
                className="p-2 text-slate-400 hover:text-rose-400 hover:bg-slate-800 rounded-lg transition"
                title="로그아웃"
              >
                <LogOut className="w-4 h-4" />
              </button>
            </div>
          </header>

          {/* 메인 업무 영역 */}
          <main className="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 space-y-6">

            {/* 통계 대시보드 */}
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {/* 조사대상 카드 */}
              <div className="bg-slate-800/40 border border-slate-700/40 p-5 rounded-2xl flex items-center justify-between shadow-sm">
                <div>
                  <span className="text-xs text-slate-400 font-bold">조사대상</span>
                  <div className="text-3xl font-black text-white mt-1">{stats.total}개</div>
                </div>
                <div className="p-3 bg-indigo-500/10 text-indigo-400 rounded-xl">
                  <Database className="w-6 h-6" />
                </div>
              </div>

              {/* 조사 완료 카드 (5개 기입 완료 항목만 집계) */}
              <div className="bg-slate-800/40 border border-slate-700/40 p-5 rounded-2xl flex items-center justify-between shadow-sm">
                <div>
                  <span className="text-xs text-slate-400 font-bold">조사 완료</span>
                  <div className="text-3xl font-black text-emerald-400 mt-1">{stats.completed}개</div>
                  <div className="w-28 bg-slate-700 h-1.5 rounded-full mt-2 overflow-hidden">
                    <div className="bg-emerald-400 h-full" style={{ width: `${stats.rate}%` }}></div>
                  </div>
                </div>
                <div className="text-right">
                  <div className="text-lg font-black text-white">{stats.rate}%</div>
                  <span className="text-[10px] text-emerald-500">실시간 완료율</span>
                </div>
              </div>
            </div>

            {/* 정합성 위반 감지 센터 배너 */}
            {validationAnomalies.length > 0 && (
              <div className="bg-rose-950/20 border border-rose-500/30 rounded-2xl p-4">
                <div className="flex items-center gap-2 text-rose-400 font-bold text-sm mb-3">
                  <AlertTriangle className="w-5 h-5 animate-pulse text-rose-500" />
                  <span>실시간 자산 정합성 오류 가이드 센터 ({validationAnomalies.length}건 규격 오류)</span>
                </div>
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3 max-h-40 overflow-y-auto pr-2 text-xs text-left">
                  {validationAnomalies.map((ano, index) => (
                    <div key={index} className="bg-slate-900/95 border border-rose-500/10 p-3 rounded-xl space-y-1">
                      <div className="flex justify-between font-bold text-slate-300">
                        <span>자산 No: {ano.assetNo}</span>
                        <span className="text-rose-500 font-mono">관리 No: {ano.mgmtNo}</span>
                      </div>
                      <p className="text-slate-400">품목: {ano.itemName} / 소속: {ano.dept}</p>
                      <div className="text-rose-400 space-y-0.5 mt-1 border-t border-slate-800 pt-1 font-mono text-[11px]">
                        {ano.errors.map((e, idx) => <p key={idx}>• {e}</p>)}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            )}

            {/* 부서별 실사 모니터링 진행 지표 */}
            <div className="bg-slate-900/60 border border-slate-800 p-4 rounded-2xl">
              <h3 className="text-xs font-bold uppercase tracking-wider text-slate-400 mb-3 flex items-center gap-2">
                <UserCheck className="w-4 h-4 text-indigo-400" />
                전사 부서별 자산 실사 진행 지표
              </h3>
              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 text-left">
                {Object.entries(stats.deptProgress).map(([key, value]) => {
                  const isCurrentUserDept = currentUser?.dept?.id === key;
                  return (
                    <div key={key} className={`p-3 rounded-xl border ${isCurrentUserDept ? 'bg-indigo-950/30 border-indigo-500/30' : 'bg-slate-950/40 border-slate-800'}`}>
                      <div className="flex justify-between items-start text-xs mb-1 gap-2">
                        <span className={`font-semibold truncate max-w-[180px] ${isCurrentUserDept ? 'text-indigo-300 font-bold' : 'text-slate-300'}`}>
                          {value.name} {isCurrentUserDept && "(내 부서)"}
                        </span>
                        
                        {value.submitted ? (
                          <span className="text-[10px] px-1.5 py-0.5 rounded bg-emerald-500/20 text-emerald-400 border border-emerald-500/30 font-bold shrink-0">
                            제출완료
                          </span>
                        ) : (
                          <span className="text-[10px] px-1.5 py-0.5 rounded bg-slate-800 text-slate-400 border border-slate-700 shrink-0 font-medium">
                            미제출
                          </span>
                        )}
                      </div>
                      
                      <div className="flex justify-between items-center text-[11px] text-slate-400 font-mono mb-1.5">
                        <span>진행율</span>
                        <span>{value.completed}/{value.total}대 ({value.rate}%)</span>
                      </div>

                      <div className="w-full bg-slate-800 h-1.5 rounded-full overflow-hidden">
                        <div 
                          className={`h-full rounded-full transition-all duration-500 ${
                            value.submitted ? 'bg-emerald-400' : value.rate > 50 ? 'bg-indigo-400' : 'bg-slate-600'
                          }`} 
                          style={{ width: `${value.rate}%` }}
                        />
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            {/* 필터 및 실사 작업자 일괄 제어 영역 */}
            <div className="bg-slate-900/80 p-5 rounded-2xl border border-slate-800 flex flex-col gap-4 text-left">
              <div className="flex flex-col lg:flex-row gap-3 justify-between items-stretch lg:items-center">
                
                {/* 실시간 대검색창 */}
                <div className="relative flex-1 max-w-sm">
                  <span className="absolute inset-y-0 left-0 flex items-center pl-3 text-slate-500">
                    <Search className="w-5 h-5" />
                  </span>
                  <input
                    type="text"
                    value={searchQuery}
                    onChange={(e) => setSearchQuery(e.target.value)}
                    placeholder="자산번호 / 관리번호 / 모델명 / 사용자 명칭 검색..."
                    className="w-full bg-slate-950 border border-slate-700/60 rounded-xl pl-10 pr-4 py-2 text-xs text-slate-200 placeholder:text-slate-600 focus:outline-none focus:ring-2 focus:ring-indigo-500"
                  />
                </div>

                {/* 실사 일괄 제어 부분 가로 한줄 배열 고정 및 스크롤 지원 */}
                <div className="flex flex-row flex-nowrap items-center gap-2 bg-slate-950/80 p-2.5 rounded-xl border border-slate-800 overflow-x-auto whitespace-nowrap shrink-0">
                  <span className="text-[10px] font-bold text-indigo-400 uppercase tracking-wide block mr-1 shrink-0">실사 일괄 제어:</span>
                  
                  {/* 실제현사용부서 일괄 입력 버튼 */}
                  <button
                    type="button"
                    disabled={isDeptSubmitted}
                    onClick={handleApplyMyDeptAll}
                    className={`px-3 py-2 rounded-lg text-xs font-semibold border transition flex items-center gap-1 shrink-0 ${
                      isDeptSubmitted
                        ? 'bg-slate-800/50 text-slate-600 border-slate-800 cursor-not-allowed'
                        : 'bg-slate-900 hover:bg-slate-850 text-indigo-300 border-indigo-500/20 cursor-pointer'
                    }`}
                    title="화면 상의 모든 미등록 자산에 내 부서 정보를 자동 반영합니다."
                  >
                    <CheckSquare className="w-3.5 h-3.5" /> 내 소속 일괄입력
                  </button>

                  {/* 임시저장 버튼 */}
                  <button
                    type="button"
                    disabled={isDeptSubmitted}
                    onClick={handleBatchSaveTemp}
                    className={`px-3 py-2 rounded-lg text-xs font-semibold transition shadow flex items-center gap-1 shrink-0 ${
                      isDeptSubmitted
                        ? 'bg-slate-800 text-slate-600 cursor-not-allowed'
                        : 'bg-amber-600 hover:bg-amber-500 text-white cursor-pointer'
                    }`}
                    title="입력한 실사 결과들을 임시 저장합니다"
                  >
                    <Save className="w-3.5 h-3.5" /> 임시저장
                  </button>

                  {/* 결과제출 버튼 */}
                  <button
                    type="button"
                    disabled={!hasTempSaved || isDeptSubmitted}
                    onClick={handleBatchSubmitFinal}
                    className={`px-3 py-2 rounded-lg text-xs font-semibold transition flex items-center gap-1 shrink-0 ${
                      hasTempSaved && !isDeptSubmitted
                        ? 'bg-emerald-600 hover:bg-emerald-500 text-white shadow cursor-pointer' 
                        : 'bg-slate-800 text-slate-500 cursor-not-allowed border border-slate-700'
                    }`}
                    title={isDeptSubmitted ? "이미 자산 조사가 최종 제출 완료되었습니다." : "임시저장 상태인 항목들을 최종 제출 확정합니다."}
                  >
                    <CheckCircle className="w-3.5 h-3.5" /> 결과제출
                  </button>

                  {/* 제출취소 버튼 */}
                  {currentUser?.dept?.id !== 'admin' && (
                    <button
                      type="button"
                      disabled={!isDeptSubmitted}
                      onClick={handleBatchCancelSubmit}
                      className={`px-3 py-2 rounded-lg text-xs font-semibold border transition flex items-center gap-1 shrink-0 ${
                        isDeptSubmitted
                          ? 'bg-rose-950/40 hover:bg-rose-900/60 text-rose-300 border-rose-500/30 cursor-pointer shadow-lg'
                          : 'bg-slate-800 text-slate-600 border-slate-800 cursor-not-allowed'
                      }`}
                      title="최종 결과제출 상태를 임시저장 상태로 해제하고 다시 수정 가능하게 만듭니다."
                    >
                      <X className="w-3.5 h-3.5 text-rose-500" /> 제출취소
                    </button>
                  )}
                </div>

                {/* 원천 대장 데이터 기입 & 엑셀 연동 */}
                <div className="flex flex-wrap items-center gap-2">
                  {currentUser?.dept?.id === 'admin' ? (
                    <>
                      <button
                        onClick={() => {
                          setFormAsset({
                            assetNo: "", 
                            mgmtNo: "",
                            itemName: "pc본체(일반형)",
                            modelName: "DB181-2(삼보)",
                            acquireDate: new Date().toISOString().substring(0, 10),
                            upperDept: "경영지원실",
                            dept: "운영지원팀-사업운영지원파트",
                            address: "",
                            addressDetail: "",
                            user: "",
                            surveyDept: "",
                            surveyUser: "",
                            purpose: "",
                            status: "",
                            statusDetail: "",
                            basis: "",
                            memo: "",
                            submitStatus: ""
                          });
                          setIsAddModalOpen(true);
                        }}
                        className="flex items-center gap-1.5 px-3 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold shadow transition"
                      >
                        <Plus className="w-3.5 h-3.5" /> 자산 수기등록 (ADMIN)
                      </button>

                      <button
                        onClick={() => setIsUploadModalOpen(true)}
                        className="flex items-center gap-1.5 px-3 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition border border-slate-700"
                      >
                        <Upload className="w-3.5 h-3.5 text-indigo-400" /> 원천 엑셀(CSV) 업로드
                      </button>

                      <button
                        onClick={setupInitialDatabase}
                        title="기초 데이터 60대 기본 복원 데이터 덮어쓰기"
                        className="p-2 bg-slate-800 hover:bg-rose-950 hover:text-rose-400 text-slate-400 rounded-xl transition border border-slate-700"
                      >
                        <RefreshCw className="w-3.5 h-3.5 text-rose-500" />
                      </button>
                    </>
                  ) : (
                    <button
                      onClick={() => setIsUploadModalOpen(true)}
                      disabled={isDeptSubmitted} 
                      className={`flex items-center gap-1.5 px-3 py-2 rounded-xl text-xs font-semibold transition border shadow-md shrink-0 ${
                        isDeptSubmitted
                          ? 'bg-slate-800 text-slate-600 border-slate-800 cursor-not-allowed shadow-none'
                          : 'bg-slate-800 hover:bg-slate-700 text-indigo-300 border-indigo-500/20 cursor-pointer'
                      }`}
                      title="엑셀(CSV)로 실사 내역을 대량 가공하여 업로드합니다."
                    >
                      <Upload className="w-3.5 h-3.5 text-indigo-400" /> 실사결과 CSV 업로드
                    </button>
                  )}

                  <button
                    onClick={handleExportCSV}
                    className="flex items-center gap-1.5 px-3 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition border border-slate-700"
                    title="현재 테이블의 조사대상을 CSV로 내보냅니다."
                  >
                    <Download className="w-3.5 h-3.5 text-emerald-400" /> CSV 파일 내보내기
                  </button>
                </div>
              </div>

              {/* 필터 세부 드롭다운 */}
              <div className="grid grid-cols-1 sm:grid-cols-3 gap-3 pt-2.5 border-t border-slate-800/80">
                <div>
                  <label className="block text-[9px] font-bold text-slate-400 uppercase tracking-wide mb-1 flex items-center gap-1">
                    <Filter className="w-3 h-3 text-indigo-400" /> 상위부서 실사필터
                  </label>
                  <select
                    value={filterUpperDept}
                    onChange={(e) => setFilterUpperDept(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-2.5 py-1.5 text-xs text-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500"
                  >
                    <option value="전체">전체 상위부서</option>
                    <option value="경영지원실">경영지원실</option>
                    <option value="GA영업부문">GA영업부문</option>
                    <option value="전속영업부문">전속영업부문</option>
                  </select>
                </div>

                <div>
                  <label className="block text-[9px] font-bold text-slate-400 uppercase tracking-wide mb-1 flex items-center gap-1">
                    <Filter className="w-3 h-3 text-indigo-400" /> 품목 필터
                  </label>
                  <select
                    value={filterItemName}
                    onChange={(e) => setFilterItemName(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-2.5 py-1.5 text-xs text-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500"
                  >
                    <option value="전체">전체 품목</option>
                    {ITEMS.map(i => <option key={i.name} value={i.name}>{i.name}</option>)}
                  </select>
                </div>

                <div>
                  <label className="block text-[9px] font-bold text-slate-400 uppercase tracking-wide mb-1 flex items-center gap-1">
                    <Filter className="w-3 h-3 text-indigo-400" /> 실사 최종여부
                  </label>
                  <select
                    value={filterSurveyStatus}
                    onChange={(e) => setFilterSurveyStatus(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-2.5 py-1.5 text-xs text-slate-300 focus:outline-none focus:ring-1 focus:ring-indigo-500"
                  >
                    <option value="전체">전체 실사목록</option>
                    <option value="완료">실사완료 (필수기입 충족)</option>
                    <option value="미조사">미완료 대상</option>
                  </select>
                </div>
              </div>
            </div>

            {/* ----------------------------------------------------------------------
                C. 자산 실사 통합 대장 리스트 테이블
                ---------------------------------------------------------------------- */}
            <div className="bg-slate-900 border border-slate-800 rounded-2xl overflow-hidden shadow-2xl">
              <div className="overflow-x-auto">
                <table className="w-full text-left border-collapse table-fixed min-w-[1750px]">
                  <thead>
                    <tr className="bg-slate-950/80 border-b border-slate-800 text-[11px] font-bold tracking-wider text-slate-400 uppercase">
                      <th className="py-4 px-3 w-[120px]">품목명</th>
                      <th className="py-4 px-3 w-[160px]">전산관리번호</th>
                      <th className="py-4 px-3 w-[190px]">자산번호</th>
                      <th className="py-4 px-3 w-[180px]">소속</th>
                      <th className="py-4 px-3 w-[150px]">모델명 / 취득일</th>
                      <th className="py-4 px-3 w-[100px]">사용자</th>
                      
                      <th className="py-4 px-3 w-[220px] bg-indigo-950/40 text-indigo-300 font-black">실제 현 사용부서 <span className="text-rose-500">*</span></th>
                      <th className="py-4 px-3 w-[120px] bg-indigo-950/40 text-indigo-300 font-black">실제 현 사용자 <span className="text-rose-500">*</span></th>
                      <th className="py-4 px-3 w-[110px] bg-indigo-950/40 text-indigo-300 font-black">용도 <span className="text-rose-500">*</span></th>
                      <th className="py-4 px-3 w-[100px] bg-indigo-950/40 text-indigo-300 font-black">자산상태 <span className="text-rose-500">*</span></th>
                      <th className="py-4 px-3 w-[120px] bg-indigo-950/40 text-indigo-300 font-black">자산상태 세부 <span className="text-rose-500">*</span></th>
                      <th className="py-4 px-3 w-[160px] bg-indigo-950/40 text-indigo-300 font-black">관련근거</th>
                      <th className="py-4 px-3 w-[160px] bg-indigo-950/40 text-indigo-300 font-black">비고</th>
                      <th className="py-4 px-3 w-[100px] text-center font-bold text-slate-400">조사상태</th>
                      {currentUser?.dept?.id === 'admin' && (
                        <th className="py-4 px-3 w-[70px] text-center text-amber-500 font-bold">관리</th>
                      )}
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-slate-800/80 text-xs text-left">
                    {filteredAssets.length === 0 ? (
                      <tr>
                        <td colSpan={currentUser?.dept?.id === 'admin' ? 15 : 14} className="py-16 text-center text-slate-500">
                          <FileText className="w-12 h-12 mx-auto mb-3 opacity-35" />
                          해당 조건에 만족하며 실사 권한에 속하는 대상 전산 기기가 존재하지 않습니다.
                        </td>
                      </tr>
                    ) : (
                      filteredAssets.map((asset) => {
                        const isSurveyValid = asset.surveyDept?.trim() && asset.surveyUser?.trim() && asset.purpose && asset.status && asset.statusDetail;
                        const isDone = asset.submitStatus === "제출완료" && isSurveyValid;
                        const isAnomaly = validationAnomalies.some(a => a.assetNo === asset.assetNo);

                        const currentEdit = rowEdits[asset.assetNo] || {};
                        const valSurveyDept = currentEdit.surveyDept !== undefined ? currentEdit.surveyDept : (asset.surveyDept || "");
                        const valSurveyUser = currentEdit.surveyUser !== undefined ? currentEdit.surveyUser : (asset.surveyUser || "");
                        const valPurpose = currentEdit.purpose !== undefined ? currentEdit.purpose : (asset.purpose || "");
                        const valStatus = currentEdit.status !== undefined ? currentEdit.status : (asset.status || "");
                        const valStatusDetail = currentEdit.statusDetail !== undefined ? currentEdit.statusDetail : (asset.statusDetail || "");
                        const valBasis = currentEdit.basis !== undefined ? currentEdit.basis : (asset.basis || "");
                        const valMemo = currentEdit.memo !== undefined ? currentEdit.memo : (asset.memo || "");

                        return (
                          <tr 
                            key={asset.assetNo} 
                            className={`hover:bg-slate-800/30 transition-colors ${
                              isDone ? 'bg-emerald-950/5' : 'bg-indigo-950/5'
                            } ${isAnomaly ? 'border-l-4 border-l-rose-500' : ''}`}
                          >
                            {/* 품목명 */}
                            <td className="py-3 px-3 font-semibold text-slate-300 truncate">
                              <span className="px-2 py-1 rounded bg-slate-950 border border-slate-700 text-[10px]">
                                {asset.itemName}
                              </span>
                            </td>

                            {/* 전산관리번호 */}
                            <td className="py-3 px-3 font-mono text-indigo-400 font-bold">
                              {asset.mgmtNo || "-"}
                            </td>

                            {/* 자산번호 */}
                            <td className="py-3 px-3 font-mono text-slate-200">
                              {asset.assetNo}
                            </td>

                            {/* 소속 */}
                            <td className="py-3 px-3 truncate">
                              <span className="text-[10px] text-slate-500 block">{asset.upperDept}</span>
                              <span className="font-medium text-slate-300">{asset.dept}</span>
                            </td>

                            {/* 모델명 / 취득일 */}
                            <td className="py-3 px-3 truncate">
                              <div className="text-slate-200 font-semibold">{asset.modelName}</div>
                              <div className="text-slate-500 text-[10px]">{asset.acquireDate}</div>
                            </td>

                            {/* 사용자 */}
                            <td className="py-3 px-3 truncate">
                              <span className="px-2 py-0.5 rounded bg-slate-950 border border-slate-800 text-slate-400">
                                {asset.user || "공용"}
                              </span>
                            </td>

                            {/* 인라인 입력 필드: 실제 현 사용부서 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <input
                                type="text"
                                disabled={isDeptSubmitted}
                                placeholder="사용부서 입력"
                                value={valSurveyDept}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'surveyDept', e.target.value)}
                                className={`w-full bg-slate-950 border focus:border-indigo-500 rounded px-2 py-1 text-xs text-white disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400 ${!valSurveyDept ? 'border-rose-500/40 bg-rose-950/5' : 'border-slate-800'}`}
                              />
                            </td>

                            {/* 인라인 입력 필드: 실제 현 사용자 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <input
                                type="text"
                                disabled={isDeptSubmitted}
                                placeholder="사용자"
                                value={valSurveyUser}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'surveyUser', e.target.value)}
                                className={`w-full bg-slate-950 border focus:border-indigo-500 rounded px-2 py-1 text-xs text-white disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400 ${!valSurveyUser ? 'border-rose-500/40 bg-rose-950/5' : 'border-slate-800'}`}
                              />
                            </td>

                            {/* 인라인 입력 필드: 용도 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <select
                                disabled={isDeptSubmitted}
                                value={valPurpose}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'purpose', e.target.value)}
                                className={`w-full bg-slate-950 border focus:border-indigo-500 rounded px-1 py-1 text-xs text-white disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400 ${!valPurpose ? 'border-rose-500/40 bg-rose-950/5' : 'border-slate-800'}`}
                              >
                                <option value="">선택</option>
                                {PURPOSES.map(p => <option key={p} value={p}>{p}</option>)}
                              </select>
                            </td>

                            {/* 인라인 입력 필드: 자산상태 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <select
                                disabled={isDeptSubmitted}
                                value={valStatus}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'status', e.target.value)}
                                className={`w-full bg-slate-950 border focus:border-indigo-500 rounded px-1 py-1 text-xs text-white font-bold disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400 ${!valStatus ? 'border-rose-500/40 bg-rose-950/5' : 'border-slate-800'}`}
                              >
                                <option value="">선택</option>
                                <option value="보유">보유</option>
                                <option value="미보유">미보유</option>
                              </select>
                            </td>

                            {/* 인라인 입력 필드: 자산상태 세부내용 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <select
                                disabled={!valStatus || isDeptSubmitted}
                                value={valStatusDetail}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'statusDetail', e.target.value)}
                                className={`w-full bg-slate-950 border focus:border-indigo-500 rounded px-1 py-1 text-xs text-white disabled:opacity-40 disabled:bg-slate-900 disabled:text-slate-400 ${!valStatusDetail ? 'border-rose-500/40 bg-rose-950/5' : 'border-slate-800'}`}
                              >
                                <option value="">선택</option>
                                {valStatus === "보유" && SURVEY_STATUS["보유"].map(sd => <option key={sd} value={sd}>{sd}</option>)}
                                {valStatus === "미보유" && SURVEY_STATUS["미보유"].map(sd => <option key={sd} value={sd}>{sd}</option>)}
                              </select>
                            </td>

                            {/* 인라인 입력 필드: 관련근거 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <input
                                type="text"
                                disabled={isDeptSubmitted}
                                placeholder="문서번호/포털 요청번호"
                                value={valBasis}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'basis', e.target.value)}
                                className="w-full bg-slate-950 border border-slate-800 focus:border-indigo-500 rounded px-2 py-1 text-xs text-white disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400"
                              />
                            </td>

                            {/* 인라인 입력 필드: 비고 */}
                            <td className="py-3 px-3 bg-indigo-950/10">
                              <input
                                type="text"
                                disabled={isDeptSubmitted}
                                placeholder="특이사항"
                                value={valMemo}
                                onChange={(e) => handleFieldChange(asset.assetNo, 'memo', e.target.value)}
                                className="w-full bg-slate-950 border border-slate-800 focus:border-indigo-500 rounded px-2 py-1 text-xs text-white disabled:opacity-50 disabled:bg-slate-900 disabled:text-slate-400"
                              />
                            </td>

                            {/* 현재 자산별 진행상태 모니터링 칩 */}
                            <td className="py-3 px-3 text-center">
                              {asset.submitStatus ? (
                                <span className={`px-2 py-0.5 rounded text-[9px] font-bold block ${
                                  asset.submitStatus === '제출완료' 
                                    ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20' 
                                    : 'bg-amber-500/10 text-amber-400 border border-amber-500/20'
                                }`}>
                                  {asset.submitStatus}
                                </span>
                              ) : (
                                <span className="px-2 py-0.5 rounded text-[9px] font-bold bg-slate-800 text-slate-400 border border-slate-700 block">
                                  미조사
                                </span>
                              )}
                            </td>

                            {/* 원천 대장 편집/삭제 (ADMIN 전용) */}
                            {currentUser?.dept?.id === 'admin' && (
                              <td className="py-3 px-3 text-center">
                                <button
                                  onClick={() => {
                                    setSelectedAsset({ ...asset });
                                    setIsEditModalOpen(true);
                                  }}
                                  className="p-1.5 text-indigo-400 hover:text-white hover:bg-indigo-600/20 rounded transition"
                                  title="기초 자산 세부내용 수동 편집"
                                >
                                  <Edit2 className="w-3.5 h-3.5" />
                                </button>
                              </td>
                            )}
                          </tr>
                        );
                      })
                    )}
                  </tbody>
                </table>
              </div>
              <div className="bg-slate-950/40 p-4 border-t border-slate-800 text-slate-400 text-xs flex justify-between items-center">
                <span>표출 대수: <strong>{filteredAssets.length}</strong> / {assets.length}대</span>
              </div>
            </div>

          </main>
        </div>
      )}

      {/* ----------------------------------------------------------------------
          D. [ADMIN 전용] 원천 DB 기초 정보 수동 수정 모달 (자산번호 강제 수정 포함)
          ---------------------------------------------------------------------- */}
      {isEditModalOpen && selectedAsset && currentUser?.dept?.id === 'admin' && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm">
          <div className="bg-slate-900 border border-slate-700/80 w-full max-w-2xl rounded-2xl overflow-hidden shadow-2xl animate-in fade-in zoom-in-95 duration-200">
            
            <div className="bg-slate-950 p-5 border-b border-slate-800 flex justify-between items-center">
              <div className="text-left">
                <h3 className="text-base font-bold text-white flex items-center gap-2">
                  <Edit2 className="w-5 h-5 text-indigo-400" />
                  원천 대장정보 강제 수정 (ADMIN 권한)
                </h3>
                <p className="text-xs text-slate-400 mt-1">대장 식별 키(구 자산번호): <span className="font-mono text-indigo-300">{selectedAsset.id || selectedAsset.assetNo}</span></p>
              </div>
              <button 
                onClick={() => setIsEditModalOpen(false)}
                className="text-slate-400 hover:text-white p-2 rounded-lg"
              >
                <X className="w-5 h-5" />
              </button>
            </div>

            <form onSubmit={handleUpdateAssetBase} className="p-6 space-y-5">
              
              <div className="bg-slate-950/50 p-4 rounded-xl border border-slate-800 grid grid-cols-2 gap-4 text-xs text-left">
                
                <div className="col-span-2 grid grid-cols-2 gap-4">
                  <div>
                    <label className="block text-[10px] font-bold text-slate-400 mb-1">자산번호 (19자리 입력)</label>
                    <input
                      type="text"
                      value={selectedAsset.assetNo || ""}
                      onChange={(e) => setSelectedAsset({ ...selectedAsset, assetNo: e.target.value })}
                      className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-indigo-400 font-mono font-bold"
                    />
                  </div>
                  <div>
                    <label className="block text-[10px] font-bold text-slate-400 mb-1">전산관리번호 (13자리)</label>
                    <input
                      type="text"
                      value={selectedAsset.mgmtNo || ""}
                      onChange={(e) => setSelectedAsset({ ...selectedAsset, mgmtNo: e.target.value })}
                      className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200 font-mono"
                    />
                  </div>
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-400 mb-1">품목명</label>
                  <select
                    value={selectedAsset.itemName}
                    onChange={(e) => setSelectedAsset({ ...selectedAsset, itemName: e.target.value })}
                    className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                  >
                    {ITEMS.map(i => <option key={i.name} value={i.name}>{i.name}</option>)}
                  </select>
                </div>
                <div>
                  <label className="block text-[10px] font-bold text-slate-400 mb-1">모델명</label>
                  <input
                    type="text"
                    value={selectedAsset.modelName || ""}
                    onChange={(e) => setSelectedAsset({ ...selectedAsset, modelName: e.target.value })}
                    className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                  />
                </div>
                <div>
                  <label className="block text-[10px] font-bold text-slate-400 mb-1">취득연월일</label>
                  <input
                    type="date"
                    value={selectedAsset.acquireDate || ""}
                    onChange={(e) => setSelectedAsset({ ...selectedAsset, acquireDate: e.target.value })}
                    className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                  />
                </div>
                <div className="col-span-2 grid grid-cols-3 gap-3">
                  <div>
                    <label className="block text-[10px] font-bold text-slate-400 mb-1">상위부서</label>
                    <input
                      type="text"
                      value={selectedAsset.upperDept || ""}
                      onChange={(e) => setSelectedAsset({ ...selectedAsset, upperDept: e.target.value })}
                      className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                    />
                  </div>
                  <div>
                    <label className="block text-[10px] font-bold text-slate-400 mb-1">사용부서</label>
                    <input
                      type="text"
                      value={selectedAsset.dept || ""}
                      onChange={(e) => setSelectedAsset({ ...selectedAsset, dept: e.target.value })}
                      className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                    />
                  </div>
                  <div>
                    <label className="block text-[10px] font-bold text-slate-400 mb-1">사용자</label>
                    <input
                      type="text"
                      value={selectedAsset.user || ""}
                      onChange={(e) => setSelectedAsset({ ...selectedAsset, user: e.target.value })}
                      className="w-full bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-slate-200"
                    />
                  </div>
                </div>
              </div>

              <div className="pt-4 border-t border-slate-800 flex justify-between items-center">
                <button
                  type="button"
                  onClick={() => handleDeleteAsset(selectedAsset.id || selectedAsset.assetNo)}
                  className="px-4 py-2 bg-rose-950/40 hover:bg-rose-900 text-rose-300 rounded-xl text-xs font-semibold transition border border-rose-500/20"
                >
                  기초 대장 영구 삭제
                </button>

                <div className="flex items-center gap-2">
                  <button
                    type="button"
                    onClick={() => setIsEditModalOpen(false)}
                    className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition"
                  >
                    취소
                  </button>
                  <button
                    type="submit"
                    className="px-5 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition"
                  >
                    관리자 권한 강제 반영
                  </button>
                </div>
              </div>

            </form>
          </div>
        </div>
      )}

      {/* ----------------------------------------------------------------------
          E. [ADMIN 전용] 새 자산 수기 등록 모달
          ---------------------------------------------------------------------- */}
      {isAddModalOpen && currentUser?.dept?.id === 'admin' && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm">
          <div className="bg-slate-900 border border-slate-700/80 w-full max-w-xl rounded-2xl overflow-hidden shadow-2xl animate-in fade-in zoom-in-95 duration-200">
            
            <div className="bg-slate-950 p-5 border-b border-slate-800 flex justify-between items-center">
              <div className="text-left">
                <h3 className="text-base font-bold text-white flex items-center gap-2">
                  <Plus className="w-5 h-5 text-indigo-400" />
                  신규 원천 자산대장 수기 등록
                </h3>
              </div>
              <button 
                onClick={() => setIsAddModalOpen(false)}
                className="text-slate-400 hover:text-white p-2 rounded-lg"
              >
                <X className="w-5 h-5" />
              </button>
            </div>

            <form onSubmit={handleCreateAsset} className="p-6 space-y-4 text-left">
              
              <div className="grid grid-cols-1 sm:grid-cols-2 gap-4 text-xs">
                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">자산번호 (19자리 입력)</label>
                  <input
                    type="text"
                    required
                    value={formAsset.assetNo}
                    onChange={(e) => setFormAsset({ ...formAsset, assetNo: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-indigo-300 font-mono font-bold"
                    placeholder="19자리 자산번호 직접 기입"
                  />
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">
                    품목명 선택
                  </label>
                  <select
                    value={formAsset.itemName}
                    onChange={(e) => {
                      const item = e.target.value;
                      const code = ITEMS.find(i => i.name === item)?.code || "T";
                      const dateStr = new Date().toISOString().substring(0, 10).replace(/-/g, '');
                      const randSeq = Math.floor(1000 + Math.random() * 9000).toString();
                      
                      let defaultModel = "DB181-2(삼보)";
                      if (item === "pc본체(대리점용)") defaultModel = "50t(레노버)";
                      else if (item === "모니터") defaultModel = "모니터(24인치)";
                      else if (item === "노트북") defaultModel = "NT951XGK(삼성)";
                      else if (item === "스캐너") defaultModel = "M160(캐논)";
                      else if (item === "프린터") defaultModel = "HL-L6415DW(브라더)";

                      setFormAsset({ 
                        ...formAsset, 
                        itemName: item,
                        modelName: defaultModel,
                        mgmtNo: `${code}${dateStr}${randSeq}` 
                      });
                    }}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                  >
                    {ITEMS.map(i => <option key={i.name} value={i.name}>{i.name}</option>)}
                  </select>
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">
                    전산관리번호
                  </label>
                  <input
                    type="text"
                    required
                    value={formAsset.mgmtNo}
                    onChange={(e) => setFormAsset({ ...formAsset, mgmtNo: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white font-mono font-semibold"
                    placeholder="T202602181001"
                  />
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">모델명 (자동 매핑 변경 가능)</label>
                  <input
                    type="text"
                    required
                    value={formAsset.modelName}
                    onChange={(e) => setFormAsset({ ...formAsset, modelName: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                  />
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">취득연월일</label>
                  <input
                    type="date"
                    required
                    value={formAsset.acquireDate}
                    onChange={(e) => setFormAsset({ ...formAsset, acquireDate: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                  />
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">사용자</label>
                  <input
                    type="text"
                    required
                    value={formAsset.user}
                    onChange={(e) => setFormAsset({ ...formAsset, user: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                    placeholder="실사용 명칭"
                  />
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">상위부서</label>
                  <select
                    value={formAsset.upperDept}
                    onChange={(e) => setFormAsset({ ...formAsset, upperDept: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                  >
                    <option value="경영지원실">경영지원실</option>
                    <option value="GA영업부문">GA영업부문</option>
                    <option value="전속영업부문">전속영업부문</option>
                  </select>
                </div>

                <div>
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">사용부서</label>
                  <input
                    type="text"
                    required
                    value={formAsset.dept}
                    onChange={(e) => setFormAsset({ ...formAsset, dept: e.target.value })}
                    className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                    placeholder="운영지원팀-HR지원파트"
                  />
                </div>

                <div className="sm:col-span-2">
                  <label className="block text-[10px] font-bold text-slate-300 mb-1">위치 정보</label>
                  <div className="grid grid-cols-2 gap-2">
                    <input
                      type="text"
                      placeholder="주소"
                      value={formAsset.address}
                      onChange={(e) => setFormAsset({ ...formAsset, address: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                    />
                    <input
                      type="text"
                      placeholder="상세주소"
                      value={formAsset.addressDetail}
                      onChange={(e) => setFormAsset({ ...formAsset, addressDetail: e.target.value })}
                      className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-white"
                    />
                  </div>
                </div>

              </div>

              <div className="pt-4 border-t border-slate-800 flex justify-end gap-2">
                <button
                  type="button"
                  onClick={() => setIsAddModalOpen(false)}
                  className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition"
                >
                  취소
                </button>
                <button
                  type="submit"
                  className="px-5 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition"
                >
                  기초 자산 등록 완료
                </button>
              </div>

            </form>
          </div>
        </div>
      )}

      {/* ----------------------------------------------------------------------
          F. 대량 엑셀(CSV) 일괄 실사결과 업로드 모달
          ---------------------------------------------------------------------- */}
      {isUploadModalOpen && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm">
          <div className="bg-slate-900 border border-slate-700/80 w-full max-w-3xl rounded-2xl overflow-hidden shadow-2xl animate-in fade-in zoom-in-95 duration-200">
            
            <div className="bg-slate-950 p-5 border-b border-slate-800 flex justify-between items-center">
              <div className="flex items-center gap-2 text-white text-left">
                <FileSpreadsheet className="w-5 h-5 text-emerald-400" />
                <div>
                  <h3 className="text-base font-bold">엑셀(CSV) 일괄 실사결과 반영</h3>
                  <p className="text-xs text-slate-400 mt-1">대량의 실사 가공 내역을 파싱하고 규격을 사전 검증하여 일괄 임시저장으로 등록합니다.</p>
                </div>
              </div>
              <button 
                onClick={() => {
                  setIsUploadModalOpen(false);
                  setCsvFeedback(null);
                  setCsvText("");
                }}
                className="text-slate-400 hover:text-white p-2 rounded-lg"
              >
                <X className="w-5 h-5" />
              </button>
            </div>

            <div className="p-6 space-y-4 text-left">
              
              <div className="text-xs bg-slate-950/80 p-4 rounded-xl border border-slate-800 space-y-2 text-left">
                <p className="font-bold text-amber-400">⚠️ 업로드 필수 헤더 양식:</p>
                <p className="text-slate-300 leading-relaxed font-mono text-[11px] bg-slate-900 p-2.5 rounded overflow-x-auto">
                  자산번호,전산관리번호,품목명,모델명,취득연월일,상위부서,사용부서,주소,상세주소,사용자,실제현사용부서,실제현사용자,용도,상태,상태세부내용,관련근거,비고
                </p>
                <p className="text-slate-400">
                  * 주의: 일반부서 담당자는 타부서 자산에 대한 일괄 실사 변경 권한이 차단되며 본인 부서 자산만 정합성 승인되어 반영됩니다.
                </p>
              </div>

              <div>
                <label className="block text-xs font-semibold text-slate-300 mb-2">CSV 데이터 텍스트 붙여넣기</label>
                <textarea
                  rows="6"
                  value={csvText}
                  onChange={(e) => setCsvText(e.target.value)}
                  placeholder={`"자산번호","전산관리번호","품목명","모델명","취득연월일","상위부서","사용부서","주소","상세주소","사용자","실제현사용부서","실제현사용자","용도","상태","상태세부내용","관련근거","비고"
"3059481101100003523","T202602180001","pc본체(일반형)","DB181-2(삼보)","2026-02-18","경영지원실","운영지원팀-사업운영지원파트","본사","7층","홍길동","경영지원실-운영지원팀-사업운영지원파트","홍길동","임직원","보유","QR등록","문서번호123","특이없음"`}
                  className="w-full bg-slate-950 border border-slate-800 rounded-xl p-3 text-slate-200 font-mono text-xs focus:outline-none focus:ring-1 focus:ring-emerald-500"
                />
              </div>

              {/* 실시간 업로드 검증 피드백 */}
              {csvFeedback && (
                <div className="bg-slate-950 p-4 rounded-xl border border-slate-800 text-xs space-y-2">
                  <div className="flex justify-between font-bold border-b border-slate-800 pb-2">
                    <span className="text-slate-300">총 레코드: {csvFeedback.total}건</span>
                    <span className="text-emerald-400">안전 통과: {csvFeedback.successCount}건</span>
                    <span className="text-rose-400">오류/차단: {csvFeedback.errorCount}</span>
                  </div>
                  <div className="max-h-32 overflow-y-auto space-y-1">
                    {csvFeedback.details.map((detail, dIdx) => (
                      <div key={dIdx} className={`p-1.5 rounded flex justify-between ${detail.critical ? 'bg-rose-950/20 text-rose-300' : 'bg-emerald-950/20 text-emerald-300'}`}>
                        <span>행 {detail.row}: {detail.mgmtNo || "자산번호오류"}</span>
                        <span className="font-mono text-[11px]">{detail.errors.join(" / ")}</span>
                      </div>
                    ))}
                  </div>
                </div>
              )}

              <div className="pt-4 border-t border-slate-800 flex justify-end gap-2">
                <button
                  type="button"
                  onClick={() => {
                    setIsUploadModalOpen(false);
                    setCsvFeedback(null);
                    setCsvText("");
                  }}
                  className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition"
                >
                  취소
                </button>
                <button
                  type="button"
                  onClick={handleUploadCSV}
                  className="px-5 py-2 bg-emerald-600 hover:bg-emerald-500 text-white rounded-xl text-xs font-semibold transition"
                >
                  정합성 검증 및 일괄 등록
                </button>
              </div>

            </div>
          </div>
        </div>
      )}

      {/* ----------------------------------------------------------------------
          G. 브라우저 Sandboxed 호환을 위한 완벽한 커스텀 모달 알림창 마크업
          ---------------------------------------------------------------------- */}
      {customModal && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm">
          <div className="bg-slate-900 border border-slate-700/80 w-full max-w-md rounded-2xl overflow-hidden shadow-2xl p-6 space-y-4 animate-in fade-in zoom-in-95 duration-150 text-left">
            <div className="flex items-center gap-3 text-white font-bold text-lg">
              <AlertCircle className="w-6 h-6 text-indigo-400" />
              <span>{customModal.title}</span>
            </div>
            <p className="text-sm text-slate-300 leading-relaxed whitespace-pre-wrap">{customModal.text}</p>
            <div className="flex justify-end gap-2 pt-2">
              {customModal.type === "confirm" && (
                <button
                  type="button"
                  onClick={() => setCustomModal(null)}
                  className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl text-xs font-semibold transition"
                >
                  취소
                </button>
              )}
              <button
                type="button"
                onClick={() => {
                  if (customModal.onConfirm) customModal.onConfirm();
                  setCustomModal(null);
                }}
                className="px-5 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-xs font-semibold transition"
              >
                확인
              </button>
            </div>
          </div>
        </div>
      )}

    </div>
  );
}