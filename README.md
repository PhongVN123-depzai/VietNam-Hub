-- VIETNAM HUB V1 - MULTI-LANGUAGE UI & MECHANICS TEMPLATE
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- 1. HỆ THỐNG ĐA NGÔN NGỮ (LOCALIZATION)
local Languages = {
    ["VI"] = {
        Title = "VIETNAM HUB V1",
        TabMain = "Chức Năng Chính",
        TabLang = "Ngôn Ngữ",
        AutoSteal = "Auto Steal (Đi Bộ Nhanh)",
        EggFinder = "Finder Egg Free (Tìm Trứng)",
        SpeedLabel = "Tốc Độ Di Chuyển:",
        StatusOn = "Trạng thái: BẬT",
        StatusOff = "Trạng thái: TẮT",
        EggFound = "Đã tìm thấy trứng tại: "
    },
    ["EN"] = {
        Title = "VIETNAM HUB V1",
        TabMain = "Main Features",
        TabLang = "Language",
        AutoSteal = "Auto Steal (Fast Walk)",
        EggFinder = "Free Egg Finder",
        SpeedLabel = "Walk Speed:",
        StatusOn = "Status: ON",
        StatusOff = "Status: OFF",
        EggFound = "Egg found at: "
    },
    ["CN"] = {
        Title = "VIETNAM HUB V1 (越南中心)",
        TabMain = "主要功能",
        TabLang = "语言设置",
        AutoSteal = "自动偷蛋 (快速移动)",
        EggFinder = "免费寻蛋器",
        SpeedLabel = "移动速度:",
        StatusOn = "状态: 开启",
        StatusOff = "状态: 关闭",
        EggFound = "发现蛋的位置: "
    },
    ["IN"] = {
        Title = "VIETNAM HUB V1",
        TabMain = "मुख्य विशेषताएं",
        TabLang = "भाषा (Language)",
        AutoSteal = "ऑटो स्टील (तेज़ चलना)",
        EggFinder = "फ्री एग फाइंडर",
        SpeedLabel = "चलने की गति:",
        StatusOn = "स्थिति: चालू",
        StatusOff = "स्थिति: बंद",
        EggFound = "अंडा यहाँ मिला: "
    }
}

local CurrentLang = "VI"
local AutoStealEnabled = false
local EggFinderEnabled = false
local CustomWalkSpeed = 50

-- 2. KHỞI TẠO GIAO DIỆN (GUI)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "VietnamHubV1"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 420, 0, 280)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -140)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

-- Thanh tiêu đề
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BackgroundColor3 = Color3.fromRGB(180, 30, 30) -- Đỏ cờ Việt Nam
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local TopCorner = Instance.new("UICorner")
TopCorner.CornerRadius = UDim.new(0, 10)
TopCorner.Parent = TopBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -50, 1, 0)
TitleLabel.Position = UDim2.new(0, 15, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = Languages[CurrentLang].Title
TitleLabel.TextColor3 = Color3.fromRGB(255, 220, 0) -- Vàng sao
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 16
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TopBar

-- Nút đóng/mở gọn
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(140, 20, 20)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = TopBar

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseBtn

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Khu vực nội dung chính
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -20, 1, -90)
ContentFrame.Position = UDim2.new(0, 10, 0, 45)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

-- Nút Auto Steal (Fast Walk)
local AutoStealBtn = Instance.new("TextButton")
AutoStealBtn.Size = UDim2.new(1, 0, 0, 45)
AutoStealBtn.Position = UDim2.new(0, 0, 0, 10)
AutoStealBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
AutoStealBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoStealBtn.Font = Enum.Font.GothamSemibold
AutoStealBtn.TextSize = 14
AutoStealBtn.Parent = ContentFrame
Instance.new("UICorner", AutoStealBtn).CornerRadius = UDim.new(0, 8)

-- Nút Finder Egg Free
local EggFinderBtn = Instance.new("TextButton")
EggFinderBtn.Size = UDim2.new(1, 0, 0, 45)
EggFinderBtn.Position = UDim2.new(0, 0, 0, 65)
EggFinderBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
EggFinderBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
EggFinderBtn.Font = Enum.Font.GothamSemibold
EggFinderBtn.TextSize = 14
EggFinderBtn.Parent = ContentFrame
Instance.new("UICorner", EggFinderBtn).CornerRadius = UDim.new(0, 8)

-- Nhãn thông báo trạng thái
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, 0, 0, 30)
StatusLabel.Position = UDim2.new(0, 0, 0, 120)
StatusLabel.BackgroundTransparency = 1
StatusLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 13
StatusLabel.Text = "..."
StatusLabel.Parent = ContentFrame

-- Thanh chọn ngôn ngữ ở dưới cùng
local LangBar = Instance.new("Frame")
LangBar.Size = UDim2.new(1, -20, 0, 35)
LangBar.Position = UDim2.new(0, 10, 1, -42)
LangBar.BackgroundTransparency = 1
LangBar.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.FillDirection = Enum.FillDirection.Horizontal
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.Padding = UDim.new(0, 8)
UIListLayout.Parent = LangBar

-- 3. CẬP NHẬT GIAO DIỆN THEO NGÔN NGỮ
local function UpdateUIText()
    local langData = Languages[CurrentLang]
    TitleLabel.Text = langData.Title
    
    local stealState = AutoStealEnabled and langData.StatusOn or langData.StatusOff
    AutoStealBtn.Text = langData.AutoSteal .. " [" .. stealState .. "]"
    AutoStealBtn.BackgroundColor3 = AutoStealEnabled and Color3.fromRGB(46, 139, 87) or Color3.fromRGB(40, 40, 48)
    
    local eggState = EggFinderEnabled and langData.StatusOn or langData.StatusOff
    EggFinderBtn.Text = langData.EggFinder .. " [" .. eggState .. "]"
    EggFinderBtn.BackgroundColor3 = EggFinderEnabled and Color3.fromRGB(46, 139, 87) or Color3.fromRGB(40, 40, 48)
end

local langCodes = {
    {Code = "VI", Label = "Tiếng Việt"},
    {Code = "EN", Label = "English"},
    {Code = "CN", Label = "中文"},
    {Code = "IN", Label = "हिन्दी"}
}

for _, info in ipairs(langCodes) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 90, 1, 0)
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
    btn.Text = info.Label
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    btn.Parent = LangBar
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    
    btn.MouseButton1Click:Connect(function()
        CurrentLang = info.Code
        UpdateUIText()
    end)
end

-- 4. LOGIC CHỨC NĂNG (MÔ PHỎNG CƠ CHẾ GAME)
-- Hàm tìm vật phẩm có tên chứa chữ "Egg" trong Workspace
local function FindNearestEgg()
    local character = LocalPlayer.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return nil end
    local rootPos = character.HumanoidRootPart.Position
    
    local nearestEgg = nil
    local shortestDistance = math.huge
    
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if (obj:IsA("BasePart") or obj:IsA("Model")) and string.find(string.lower(obj.Name), "egg") then
            local targetPart = obj:IsA("Model") and (obj.PrimaryPart or obj:FindFirstChildWhichIsA("BasePart")) or obj
            if targetPart then
                local dist = (targetPart.Position - rootPos).Magnitude
                if dist < shortestDistance then
                    shortestDistance = dist
                    nearestEgg = targetPart
                end
            end
        end
    end
    return nearestEgg, shortestDistance
end

-- Tạo Highlight (ESP) cho trứng khi bật Egg Finder
local function ToggleEggESP(state)
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if string.find(string.lower(obj.Name), "egg") then
            local existingHighlight = obj:FindFirstChild("EggESP_Highlight")
            if state then
                if not existingHighlight then
                    local hl = Instance.new("Highlight")
                    hl.Name = "EggESP_Highlight"
                    hl.FillColor = Color3.fromRGB(255, 215, 0)
                    hl.OutlineColor = Color3.fromRGB(255, 255, 255)
                    hl.Parent = obj
                end
            else
                if existingHighlight then
                    existingHighlight:Destroy()
                end
            end
        end
    end
end

-- Sự kiện bấm nút Auto Steal (Đi bộ nhanh tới trứng)
AutoStealBtn.MouseButton1Click:Connect(function()
    AutoStealEnabled = not AutoStealEnabled
    UpdateUIText()
    
    local character = LocalPlayer.Character
    local humanoid = character and character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = AutoStealEnabled and CustomWalkSpeed or 16
    end
end)

-- Sự kiện bấm nút Egg Finder
EggFinderBtn.MouseButton1Click:Connect(function()
    EggFinderEnabled = not EggFinderEnabled
    UpdateUIText()
    ToggleEggESP(EggFinderEnabled)
    
    if EggFinderEnabled then
        local egg, dist = FindNearestEgg()
        if egg then
            StatusLabel.Text = Languages[CurrentLang].EggFound .. egg.Name .. " (" .. math.floor(dist) .. "m)"
        else
            StatusLabel.Text = "No Egg found in Workspace."
        end
    else
        StatusLabel.Text = "..."
    end
end)

-- Vòng lặp di chuyển nhanh đến trứng khi bật Auto Steal
RunService.Heartbeat:Connect(function()
    if AutoStealEnabled then
        local character = LocalPlayer.Character
        local humanoid = character and character:FindFirstChildWhichIsA("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = CustomWalkSpeed
            local targetEgg = FindNearestEgg()
            if targetEgg then
                humanoid:MoveTo(targetEgg.Position)
            end
        end
    end
end)

-- Khởi tạo văn bản ban đầu
UpdateUIText()
