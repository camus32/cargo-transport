# 货物运输分配工具 - 技术规格文档

---

## 组件清单

### shadcn/ui 组件

- **Button** - 按钮
- **Card** - 卡片容器
- **Dialog** - 对话框
- **Input** - 输入框
- **Badge** - 标签
- **Progress** - 进度条
- **ScrollArea** - 滚动区域
- **Separator** - 分隔线
- **Tooltip** - 提示框

### 自定义组件

- **CargoList** - 货物清单面板
- **CargoItem** - 货物列表项
- **VehicleCard** - 车辆卡片
- **VehicleCargoList** - 车辆货物列表
- **AddCargoDialog** - 添加货物对话框
- **StatisticsPanel** - 统计面板
- **SearchInput** - 搜索输入框

---

## 动画实现计划

| 动画效果 | 实现方式 | 复杂度 |
|---------|---------|--------|
| 页面加载淡入 | CSS transition | 低 |
| 卡片悬停阴影 | CSS transition | 低 |
| 展开/收起 | CSS max-height transition | 中 |
| 数字变化 | React state + CSS | 低 |
| 对话框显示/隐藏 | shadcn Dialog 内置 | 低 |
| 列表项添加/删除 | CSS animation | 中 |

---

## 项目文件结构

```
src/
├── components/
│   ├── ui/                    # shadcn/ui 组件
│   ├── CargoList.tsx          # 货物清单面板
│   ├── CargoItem.tsx          # 货物列表项
│   ├── VehicleCard.tsx        # 车辆卡片
│   ├── AddCargoDialog.tsx     # 添加货物对话框
│   ├── StatisticsPanel.tsx    # 统计面板
│   └── SearchInput.tsx        # 搜索输入框
├── hooks/
│   └── useCargoState.ts       # 货物状态管理 Hook
├── types/
│   └── index.ts               # TypeScript 类型定义
├── data/
│   └── cargoData.ts           # 货物数据
├── utils/
│   └── helpers.ts             # 工具函数
├── App.tsx                    # 主应用组件
├── App.css                    # 应用样式
└── main.tsx                   # 入口文件
```

---

## 依赖项

- React 18+
- TypeScript
- Tailwind CSS
- shadcn/ui 组件库
- Lucide React (图标)
- xlsx (Excel解析 - 已预装)

---

## 状态管理

使用 React useState + useContext 进行状态管理:

```typescript
// 全局状态
const [cargos, setCargos] = useState<Cargo[]>(initialCargos);
const [vehicles, setVehicles] = useState<Vehicle[]>([]);

// 派生状态
const remainingCargos = useMemo(() => {...}, [cargos, vehicles]);
const vehicleStats = useMemo(() => {...}, [vehicles, cargos]);
```

---

## 核心算法

### 计算货物剩余数量

```typescript
function getRemainingQuantity(cargo: Cargo, vehicles: Vehicle[]): number {
  const allocated = vehicles.reduce((sum, vehicle) => {
    const vc = vehicle.cargos.find(vc => vc.cargoId === cargo.id);
    return sum + (vc?.quantity || 0);
  }, 0);
  return cargo.totalQuantity - allocated;
}
```

### 计算车辆总重量

```typescript
function getVehicleTotalWeight(vehicle: Vehicle, cargos: Cargo[]): number {
  return vehicle.cargos.reduce((sum, vc) => {
    const cargo = cargos.find(c => c.id === vc.cargoId);
    return sum + (cargo?.grossWeight || 0) * vc.quantity;
  }, 0);
}
```
