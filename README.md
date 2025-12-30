-- ===========================================
-- MrYesHackk Script | Ultimate Edition v3.0
-- ===========================================
-- Developer: MrYesHackk
-- Version: 3.0 Premium
-- ===========================================

local Players = game:GetService("Players")
local TCS = game:GetService("TextChatService")
local VIM = game:GetService("VirtualInputManager")
local Workspace = game:GetService("Workspace")
local player = Players.LocalPlayer
local character = nil

-- تأخير لضمان تحميل الشخصية
task.spawn(function()
    while not player.Character do
        task.wait()
    end
    character = player.Character
end)

-- ===========================================
-- متغيرات النظام
-- ===========================================
local AntiLagLevel = 0
local AntiAFK = false
local NoChat = false
local HDActive = false
local ChatSpamActive = false
local CommandSpamActive = false
local BangActive = false
local HeadBangActive = false
local SelectedPlayer = nil
local afkConnection = nil
local spamTasks = {}
local bangTask = nil
local commandHistory = {}
local chatHistory = {}
local savedMessages = {}

-- ===========================================
-- إعدادات الذاكرة
-- ===========================================
local function CleanMemory()
    collectgarbage()
    print("🧹 تم تنظيف الذاكرة")
end

-- ===========================================
-- مضاد لاق (ANTI LAG) - 6 مستويات متقدمة
-- ===========================================
local function AntiLag_Level1()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level03
    settings().Rendering.EnableFRM = false
    
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
        end
    end
    print("✅ Anti Lag: Level 1 - إعدادات أساسية")
end

local function AntiLag_Level2()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    settings().Rendering.EnableFRM = false
    
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Trail") then
            v.Enabled = false
        end
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
        end
    end
    print("✅ Anti Lag: Level 2 - تعطيل الجسيمات")
end

local function AntiLag_Level3()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    settings().Rendering.EnableFRM = false
    
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter")
        or v:IsA("Trail")
        or v:IsA("Smoke")
        or v:IsA("Fire")
        or v:IsA("Explosion")
        or v:IsA("Sparkles") then
            v.Enabled = false
        end
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
        end
        if v:IsA("Decal") or v:IsA("Texture") then
            v:Destroy()
        end
    end
    print("✅ Anti Lag: Level 3 - إزالة الديكور")
end

local function AntiLag_Level4()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    settings().Rendering.EnableFRM = false
    settings().Physics.ThrottleAdjustTime = 2
    
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter")
        or v:IsA("Trail")
        or v:IsA("Smoke")
        or v:IsA("Fire")
        or v:IsA("Explosion")
        or v:IsA("Sparkles")
        or v:IsA("Beam") then
            v.Enabled = false
        end
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
            v.CastShadow = false
        end
        if v:IsA("Decal") or v:IsA("Texture") then
            v:Destroy()
        end
        if v:IsA("SurfaceGui") or v:IsA("SurfaceLight") then
            v.Enabled = false
        end
    end
    print("✅ Anti Lag: Level 4 - تحسينات متقدمة")
end

local function AntiLag_Level5()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    settings().Rendering.EnableFRM = false
    settings().Physics.ThrottleAdjustTime = 1
    settings().Rendering.EagerBulkExecution = true
    
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter")
        or v:IsA("Trail")
        or v:IsA("Smoke")
        or v:IsA("Fire")
        or v:IsA("Explosion")
        or v:IsA("Sparkles")
        or v:IsA("Beam")
        or v:IsA("PointLight")
        or v:IsA("SpotLight") then
            v.Enabled = false
        end
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
            v.CastShadow = false
            v.Anchored = false
        end
        if v:IsA("Decal") or v:IsA("Texture") or v:IsA("MeshPart") then
            v:Destroy()
        end
        if v:IsA("SurfaceGui") or v:IsA("SurfaceLight") or v:IsA("BillboardGui") then
            v.Enabled = false
        end
    end
    
    -- تقليل جودة الإضاءة
    game:GetService("Lighting").GlobalShadows = false
    game:GetService("Lighting").Technology = Enum.Technology.Compatibility
    
    print("✅ Anti Lag: Level 5 - مستوى متقدم")
end

local function AntiLag_Level6()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    settings().Rendering.EnableFRM = false
    settings().Physics.ThrottleAdjustTime = 0.5
    settings().Rendering.EagerBulkExecution = true
    
    -- تعطيل كل التأثيرات البصرية
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter")
        or v:IsA("Trail")
        or v:IsA("Smoke")
        or v:IsA("Fire")
        or v:IsA("Explosion")
        or v:IsA("Sparkles")
        or v:IsA("Beam")
        or v:IsA("PointLight")
        or v:IsA("SpotLight")
        or v:IsA("SurfaceLight") then
            v.Enabled = false
        end
        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
            v.CastShadow = false
            v.Anchored = false
            v.Transparency = 0
        end
        if v:IsA("Decal") 
        or v:IsA("Texture") 
        or v:IsA("MeshPart")
        or v:IsA("UnionOperation") then
            v:Destroy()
        end
        if v:IsA("SurfaceGui") 
        or v:IsA("BillboardGui")
        or v:IsA("ScreenGui") then
            v.Enabled = false
        end
        if v:IsA("Sound") then
            v:Destroy()
        end
    end
    
    -- إعدادات الإضاءة المتطرفة
    local lighting = game:GetService("Lighting")
    lighting.GlobalShadows = false
    lighting.Technology = Enum.Technology.Compatibility
    lighting.Brightness = 1
    lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
    lighting.ClockTime = 12
    lighting.GeographicLatitude = 0
    
    -- إزالة السماء
    if lighting:FindFirstChild("Sky") then
        lighting.Sky:Destroy()
    end
    
    print("✅ Anti Lag: Level 6 - MAXIMUM (أدنى إستهلاك)")
end

-- ===========================================
-- نظام الأوامر (محسن للسرعة الفائقة)
-- ===========================================
local function ExecuteCommand(cmd)
    if cmd and cmd ~= "" then
        pcall(function()
            game:GetService("ReplicatedStorage").HDAdminHDClient.Signals.RequestCommandSilent:InvokeServer(cmd)
            
            table.insert(commandHistory, 1, cmd)
            if #commandHistory > 15 then
                table.remove(commandHistory, 16)
            end
            
            print("✅ الأمر تم تنفيذه: " .. cmd)
            return true
        end)
    end
    return false
end

-- ===========================================
-- نظام سبام الأوامر الفائق السرعة
-- ===========================================
local function StartUltraCommandSpam(command)
    if CommandSpamActive then return end
    CommandSpamActive = true
    
    local taskId = #spamTasks + 1
    spamTasks[taskId] = task.spawn(function()
        while CommandSpamActive do
            pcall(function()
                game:GetService("ReplicatedStorage").HDAdminHDClient.Signals.RequestCommandSilent:InvokeServer(command)
            end)
            -- بدون أي انتظار بين الأوامر (أقصى سرعة)
        end
        spamTasks[taskId] = nil
    end)
end

local function StopCommandSpam()
    CommandSpamActive = false
    for _, task in pairs(spamTasks) do
        task.cancel(task)
    end
    spamTasks = {}
end

-- ===========================================
-- نظام الشات المحسن
-- ===========================================
local function SendChatMessage(message)
    if message and message ~= "" then
        pcall(function()
            table.insert(chatHistory, 1, message)
            if #chatHistory > 15 then
                table.remove(chatHistory, 16)
            end
            
            local success = false
            
            -- محاولة العثور على RemoteEvent للشات
            local function FindChatRemote()
                local events = game:GetService("ReplicatedStorage"):FindFirstChild("Events")
                if events then
                    local chatRemote = events:FindFirstChild("SendMessage")
                    if chatRemote then
                        pcall(function() chatRemote:FireServer(message) end)
                        pcall(function() chatRemote:FireServer(player, message) end)
                        pcall(function() chatRemote:FireServer({Text = message}) end)
                        return true
                    end
                end
                
                for _, obj in pairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
                    if obj:IsA("RemoteEvent") then
                        if obj.Name:lower():find("chat") or obj.Name:lower():find("message") or obj.Name:lower():find("send") then
                            pcall(function() obj:FireServer(message) end)
                            return true
                        end
                    end
                end
                
                return false
            end
            
            success = FindChatRemote()
            
            if not success then
                pcall(function()
                    local chatEvents = game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents")
                    if chatEvents then
                        local sayMessage = chatEvents:FindFirstChild("SayMessageRequest")
                        if sayMessage then
                            sayMessage:FireServer(message, "All")
                            success = true
                        end
                    end
                end)
            end
            
            if not success then
                pcall(function()
                    if TCS.TextChannels then
                        local textChannel = TCS.TextChannels:FindFirstChild("RBXGeneral")
                        if textChannel then
                            textChannel:SendAsync(message)
                            success = true
                        end
                    end
                end)
            end
            
            if success then
                print("✅ تم إرسال الرسالة: " .. message)
            end
            
            return success
        end)
    end
    return false
end

local function StartChatSpam(message, speed)
    if ChatSpamActive then return end
    ChatSpamActive = true
    
    local taskId = #spamTasks + 1
    spamTasks[taskId] = task.spawn(function()
        while ChatSpamActive do
            SendChatMessage(message)
            
            local waitTime = speed or 1
            if waitTime < 0.000000000001 then
                waitTime = 0.000000000001
            end
            
            if waitTime < 0.001 then
                local endTime = tick() + waitTime
                while tick() < endTime and ChatSpamActive do
                    task.wait()
                end
            else
                task.wait(waitTime)
            end
        end
        spamTasks[taskId] = nil
    end)
end

local function StopChatSpam()
    ChatSpamActive = false
    for _, task in pairs(spamTasks) do
        task.cancel(task)
    end
    spamTasks = {}
end

-- ===========================================
-- نظام البحث عن اللاعبين
-- ===========================================
local function FindPlayer(searchTerm)
    local foundPlayers = {}
    
    for _, targetPlayer in pairs(Players:GetPlayers()) do
        if targetPlayer ~= player then
            local searchLower = searchTerm:lower()
            local nameLower = targetPlayer.Name:lower()
            local displayNameLower = targetPlayer.DisplayName:lower()
            local userIdStr = tostring(targetPlayer.UserId)
            
            if nameLower:find(searchLower) 
            or displayNameLower:find(searchLower)
            or userIdStr:find(searchTerm) then
                table.insert(foundPlayers, targetPlayer)
            end
        end
    end
    
    return foundPlayers
end

local function GetPlayerInfo(targetPlayer)
    local info = ""
    info = info .. "👤 الاسم: " .. targetPlayer.Name .. "\n"
    info = info .. "🎭 العرض: " .. targetPlayer.DisplayName .. "\n"
    info = info .. "🆔 ID: " .. tostring(targetPlayer.UserId) .. "\n"
    
    -- حساب عمر الحساب
    pcall(function()
        local success, accountAge = pcall(function()
            return targetPlayer.AccountAge
        end)
        
        if success and accountAge then
            local days = accountAge
            local years = math.floor(days / 365)
            local remainingDays = days % 365
            local months = math.floor(remainingDays / 30)
            
            if years > 0 then
                info = info .. "📅 العمر: " .. years .. " سنة"
                if months > 0 then
                    info = info .. " و " .. months .. " شهر"
                end
            elseif months > 0 then
                info = info .. "📅 العمر: " .. months .. " شهر"
            else
                info = info .. "📅 العمر: " .. days .. " يوم"
            end
        end
    end)
    
    return info
end

-- ===========================================
-- نظام الرقصات (بانج وهيدبانج)
-- ===========================================
local function StartBang(targetPlayer)
    if BangActive or not targetPlayer then return end
    BangActive = true
    
    bangTask = task.spawn(function()
        local danceAnimation = nil
        
        -- تحميل الرقصة
        pcall(function()
            danceAnimation = Instance.new("Animation")
            danceAnimation.AnimationId = "rbxassetid://5918726674"
        end)
        
        while BangActive and targetPlayer and targetPlayer.Parent do
            pcall(function()
                if player.Character and targetPlayer.Character then
                    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                    local targetPos = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                    
                    if humanoid and targetPos then
                        -- التحرك خلف اللاعب
                        local behindPosition = targetPos.Position - targetPos.CFrame.LookVector * 3
                        player.Character:MoveTo(behindPosition)
                        
                        -- تشغيل الرقصة
                        if danceAnimation then
                            local animator = humanoid:FindFirstChildOfClass("Animator")
                            if animator then
                                local animationTrack = animator:LoadAnimation(danceAnimation)
                                animationTrack:Play()
                            end
                        end
                    end
                end
            end)
            task.wait(0.1)
        end
        
        BangActive = false
    end)
end

local function StartHeadBang(targetPlayer)
    if HeadBangActive or not targetPlayer then return end
    HeadBangActive = true
    
    bangTask = task.spawn(function()
        local danceAnimation = nil
        
        pcall(function()
            danceAnimation = Instance.new("Animation")
            danceAnimation.AnimationId = "rbxassetid://5918726674"
        end)
        
        while HeadBangActive and targetPlayer and targetPlayer.Parent do
            pcall(function()
                if player.Character and targetPlayer.Character then
                    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                    local targetPos = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                    
                    if humanoid and targetPos then
                        -- التحرك أمام اللاعب
                        local frontPosition = targetPos.Position + targetPos.CFrame.LookVector * 3
                        player.Character:MoveTo(frontPosition)
                        
                        -- الدوران لمواجهة اللاعب
                        local lookAt = targetPos.Position - player.Character.HumanoidRootPart.Position
                        player.Character.HumanoidRootPart.CFrame = CFrame.new(
                            player.Character.HumanoidRootPart.Position,
                            player.Character.HumanoidRootPart.Position + Vector3.new(lookAt.X, 0, lookAt.Z)
                        )
                        
                        -- تشغيل الرقصة
                        if danceAnimation then
                            local animator = humanoid:FindFirstChildOfClass("Animator")
                            if animator then
                                local animationTrack = animator:LoadAnimation(danceAnimation)
                                animationTrack:Play()
                            end
                        end
                    end
                end
            end)
            task.wait(0.1)
        end
        
        HeadBangActive = false
    end)
end

local function StopBang()
    BangActive = false
    HeadBangActive = false
    if bangTask then
        task.cancel(bangTask)
        bangTask = nil
    end
end

-- ===========================================
-- نظام Anti AFK
-- ===========================================
local function EnableAntiAFK()
    if AntiAFK then return end
    AntiAFK = true

    afkConnection = player.Idled:Connect(function()
        VIM:SendKeyEvent(true, "W", false, game)
        task.wait(0.15)
        VIM:SendKeyEvent(false, "W", false, game)
    end)

    task.spawn(function()
        while AntiAFK do
            task.wait(45)
            pcall(function()
                local hum = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
                if hum then
                    hum:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end)
        end
    end)
    print("✅ Anti AFK: مفعل")
end

local function DisableAntiAFK()
    AntiAFK = false
    if afkConnection then
        afkConnection:Disconnect()
        afkConnection = nil
    end
    print("❌ Anti AFK: متوقف")
end

-- ===========================================
-- واجهة MrYesHackk الرئيسية (مع التحديثات الجديدة)
-- ===========================================
if game.CoreGui:FindFirstChild("MrYesHackkGUI") then
    game.CoreGui.MrYesHackkGUI:Destroy()
end

-- إنشاء الواجهة الرئيسية
local mainGUI = Instance.new("ScreenGui", game.CoreGui)
mainGUI.Name = "MrYesHackkGUI"
mainGUI.ResetOnSpawn = false
mainGUI.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- الإطار الرئيسي
local mainFrame = Instance.new("Frame", mainGUI)
mainFrame.Size = UDim2.new(0, 380, 0, 550) -- زيادة الارتفاع لاستيعاب الميزات الجديدة
mainFrame.Position = UDim2.new(0.5, -190, 0.5, -275)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 20, 30)
mainFrame.Active = true
mainFrame.Draggable = true

-- تأثير الخلفية المتدرجة
local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 25, 40)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(10, 15, 25))
})
gradient.Rotation = 90
gradient.Parent = mainFrame

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 15)
mainCorner.Parent = mainFrame

local mainStroke = Instance.new("UIStroke")
mainStroke.Color = Color3.fromRGB(0, 255, 255)
mainStroke.Thickness = 2
mainStroke.Transparency = 0.3
mainStroke.Parent = mainFrame

-- شريط العنوان
local titleBar = Instance.new("Frame", mainFrame)
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 30, 45)
titleBar.BackgroundTransparency = 0.3

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 15)
titleCorner.Parent = titleBar

local titleLabel = Instance.new("TextLabel", titleBar)
titleLabel.Size = UDim2.new(1, 0, 1, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "👑 MrYesHackk v3.0"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 22
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextStrokeTransparency = 0.7
titleLabel.TextStrokeColor3 = Color3.fromRGB(0, 150, 255)

-- زر الإغلاق
local closeButton = Instance.new("TextButton", titleBar)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(0.92, 0, 0.1, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
closeButton.Text = "✕"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 18
closeButton.Font = Enum.Font.GothamBold

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 8)
closeCorner.Parent = closeButton

-- منطقة المحتوى
local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, -20, 1, -70)
contentFrame.Position = UDim2.new(0, 10, 0, 60)
contentFrame.BackgroundTransparency = 1

-- إنشاء علامات التبويب (زيادة علامة تبويب)
local tabButtonsFrame = Instance.new("Frame", contentFrame)
tabButtonsFrame.Size = UDim2.new(1, 0, 0, 35)
tabButtonsFrame.BackgroundTransparency = 1

local tabs = {}
local currentTab = "main"

-- دالة إنشاء زر علامة تبويب
local function CreateTabButton(name, text, xPosition)
    local tabButton = Instance.new("TextButton", tabButtonsFrame)
    tabButton.Size = UDim2.new(0.19, 0, 1, 0) -- أصغر لاستيعاب علامات تبويب أكثر
    tabButton.Position = UDim2.new(xPosition, 0, 0, 0)
    tabButton.BackgroundColor3 = Color3.fromRGB(40, 45, 65)
    tabButton.Text = text
    tabButton.TextColor3 = Color3.fromRGB(200, 200, 255)
    tabButton.TextSize = 12
    tabButton.Font = Enum.Font.GothamBold
    tabButton.Name = name
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 8)
    tabCorner.Parent = tabButton
    
    local tabStroke = Instance.new("UIStroke")
    tabStroke.Color = Color3.fromRGB(0, 200, 255)
    tabStroke.Thickness = 1
    tabStroke.Parent = tabButton
    
    tabs[name] = Instance.new("Frame", contentFrame)
    tabs[name].Size = UDim2.new(1, 0, 1, -45)
    tabs[name].Position = UDim2.new(0, 0, 0, 40)
    tabs[name].BackgroundTransparency = 1
    tabs[name].Visible = false
    
    return tabButton
end

-- إنشاء علامات التبويب (5 علامات)
local mainTabBtn = CreateTabButton("main", "🏠", 0)
local playerTabBtn = CreateTabButton("player", "👤", 0.19) -- علامة تبويب جديدة للاعب
local commandsTabBtn = CreateTabButton("commands", "🎮", 0.38)
local chatTabBtn = CreateTabButton("chat", "💬", 0.57)
local toolsTabBtn = CreateTabButton("tools", "⚙️", 0.76)

-- منطقة المحتوى الرئيسية
tabs.main.Visible = true

-- دالة تبديل علامات التبويب
local function SwitchTab(tabName)
    currentTab = tabName
    for name, tab in pairs(tabs) do
        tab.Visible = (name == tabName)
        local btn = tabButtonsFrame:FindFirstChild(name)
        if btn then
            if name == tabName then
                btn.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
                btn.TextColor3 = Color3.fromRGB(255, 255, 255)
                btn.UIStroke.Color = Color3.fromRGB(255, 255, 255)
            else
                btn.BackgroundColor3 = Color3.fromRGB(40, 45, 65)
                btn.TextColor3 = Color3.fromRGB(200, 200, 255)
                btn.UIStroke.Color = Color3.fromRGB(0, 200, 255)
            end
        end
    end
end

-- ===========================================
-- علامة التبويب الرئيسية (مع Anti Lag الجديد)
-- ===========================================
local mainContent = tabs.main

-- بطاقة Anti Lag (محدثة مع 6 مستويات)
local antiLagCard = Instance.new("Frame", mainContent)
antiLagCard.Size = UDim2.new(1, -10, 0, 90)
antiLagCard.Position = UDim2.new(0, 5, 0, 10)
antiLagCard.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local cardCorner1 = Instance.new("UICorner")
cardCorner1.CornerRadius = UDim.new(0, 12)
cardCorner1.Parent = antiLagCard

local antiLagTitle = Instance.new("TextLabel", antiLagCard)
antiLagTitle.Size = UDim2.new(0.6, 0, 0, 30)
antiLagTitle.Position = UDim2.new(0, 10, 0, 10)
antiLagTitle.BackgroundTransparency = 1
antiLagTitle.Text = "🚀 Anti Lag (6 مستويات)"
antiLagTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
antiLagTitle.TextSize = 16
antiLagTitle.Font = Enum.Font.GothamBold
antiLagTitle.TextXAlignment = Enum.TextXAlignment.Left

local antiLagLevel = Instance.new("TextLabel", antiLagCard)
antiLagLevel.Size = UDim2.new(0.5, 0, 0, 20)
antiLagLevel.Position = UDim2.new(0, 10, 0, 45)
antiLagLevel.BackgroundTransparency = 1
antiLagLevel.Text = "المستوى: 0/6"
antiLagLevel.TextColor3 = Color3.fromRGB(200, 200, 255)
antiLagLevel.TextSize = 12
antiLagLevel.Font = Enum.Font.Gotham
antiLagLevel.TextXAlignment = Enum.TextXAlignment.Left

local antiLagButton = Instance.new("TextButton", antiLagCard)
antiLagButton.Size = UDim2.new(0, 90, 0, 30)
antiLagButton.Position = UDim2.new(1, -100, 0, 25)
antiLagButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
antiLagButton.Text = "المستوى 1"
antiLagButton.TextColor3 = Color3.fromRGB(255, 255, 255)
antiLagButton.TextSize = 12
antiLagButton.Font = Enum.Font.GothamBold

local buttonCorner1 = Instance.new("UICorner")
buttonCorner1.CornerRadius = UDim.new(0, 6)
buttonCorner1.Parent = antiLagButton

-- بطاقة Anti AFK
local antiAFKCard = Instance.new("Frame", mainContent)
antiAFKCard.Size = UDim2.new(1, -10, 0, 70)
antiAFKCard.Position = UDim2.new(0, 5, 0, 110)
antiAFKCard.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local cardCorner2 = Instance.new("UICorner")
cardCorner2.CornerRadius = UDim.new(0, 12)
cardCorner2.Parent = antiAFKCard

local antiAFKTitle = Instance.new("TextLabel", antiAFKCard)
antiAFKTitle.Size = UDim2.new(0.6, 0, 0, 30)
antiAFKTitle.Position = UDim2.new(0, 10, 0, 10)
antiAFKTitle.BackgroundTransparency = 1
antiAFKTitle.Text = "🛡️ Anti AFK"
antiAFKTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
antiAFKTitle.TextSize = 16
antiAFKTitle.Font = Enum.Font.GothamBold
antiAFKTitle.TextXAlignment = Enum.TextXAlignment.Left

local antiAFKStatus = Instance.new("TextLabel", antiAFKCard)
antiAFKStatus.Size = UDim2.new(0.5, 0, 0, 20)
antiAFKStatus.Position = UDim2.new(0, 10, 0, 40)
antiAFKStatus.BackgroundTransparency = 1
antiAFKStatus.Text = "❌ متوقف"
antiAFKStatus.TextColor3 = Color3.fromRGB(200, 200, 255)
antiAFKStatus.TextSize = 12
antiAFKStatus.Font = Enum.Font.Gotham
antiAFKStatus.TextXAlignment = Enum.TextXAlignment.Left

local antiAFKButton = Instance.new("TextButton", antiAFKCard)
antiAFKButton.Size = UDim2.new(0, 80, 0, 30)
antiAFKButton.Position = UDim2.new(1, -90, 0, 20)
antiAFKButton.BackgroundColor3 = Color3.fromRGB(60, 180, 100)
antiAFKButton.Text = "تفعيل"
antiAFKButton.TextColor3 = Color3.fromRGB(255, 255, 255)
antiAFKButton.TextSize = 12
antiAFKButton.Font = Enum.Font.GothamBold

local buttonCorner2 = Instance.new("UICorner")
buttonCorner2.CornerRadius = UDim.new(0, 6)
buttonCorner2.Parent = antiAFKButton

-- بطاقة Remove Chat
local removeChatCard = Instance.new("Frame", mainContent)
removeChatCard.Size = UDim2.new(1, -10, 0, 70)
removeChatCard.Position = UDim2.new(0, 5, 0, 190)
removeChatCard.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local cardCorner3 = Instance.new("UICorner")
cardCorner3.CornerRadius = UDim.new(0, 12)
cardCorner3.Parent = removeChatCard

local removeChatTitle = Instance.new("TextLabel", removeChatCard)
removeChatTitle.Size = UDim2.new(0.6, 0, 0, 30)
removeChatTitle.Position = UDim2.new(0, 10, 0, 10)
removeChatTitle.BackgroundTransparency = 1
removeChatTitle.Text = "💬 إخفاء الشات"
removeChatTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
removeChatTitle.TextSize = 16
removeChatTitle.Font = Enum.Font.GothamBold
removeChatTitle.TextXAlignment = Enum.TextXAlignment.Left

local removeChatStatus = Instance.new("TextLabel", removeChatCard)
removeChatStatus.Size = UDim2.new(0.5, 0, 0, 20)
removeChatStatus.Position = UDim2.new(0, 10, 0, 40)
removeChatStatus.BackgroundTransparency = 1
removeChatStatus.Text = "❌ متوقف"
removeChatStatus.TextColor3 = Color3.fromRGB(200, 200, 255)
removeChatStatus.TextSize = 12
removeChatStatus.Font = Enum.Font.Gotham
removeChatStatus.TextXAlignment = Enum.TextXAlignment.Left

local removeChatButton = Instance.new("TextButton", removeChatCard)
removeChatButton.Size = UDim2.new(0, 80, 0, 30)
removeChatButton.Position = UDim2.new(1, -90, 0, 20)
removeChatButton.BackgroundColor3 = Color3.fromRGB(180, 80, 60)
removeChatButton.Text = "تفعيل"
removeChatButton.TextColor3 = Color3.fromRGB(255, 255, 255)
removeChatButton.TextSize = 12
removeChatButton.Font = Enum.Font.GothamBold

local buttonCorner3 = Instance.new("UICorner")
buttonCorner3.CornerRadius = UDim.new(0, 6)
buttonCorner3.Parent = removeChatButton

-- ===========================================
-- علامة تبويب اللاعب (جديدة)
-- ===========================================
local playerContent = tabs.player

-- حقل البحث عن لاعب
local searchFrame = Instance.new("Frame", playerContent)
searchFrame.Size = UDim2.new(1, -10, 0, 90)
searchFrame.Position = UDim2.new(0, 5, 0, 10)
searchFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local searchCorner = Instance.new("UICorner")
searchCorner.CornerRadius = UDim.new(0, 12)
searchCorner.Parent = searchFrame

local searchBox = Instance.new("TextBox", searchFrame)
searchBox.Size = UDim2.new(1, -20, 0, 40)
searchBox.Position = UDim2.new(0, 10, 0, 10)
searchBox.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
searchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
searchBox.PlaceholderText = "اسم، اسم العرض، أو ID"
searchBox.Text = ""
searchBox.TextSize = 14
searchBox.Font = Enum.Font.Gotham
searchBox.ClearTextOnFocus = false

local searchBoxCorner = Instance.new("UICorner")
searchBoxCorner.CornerRadius = UDim.new(0, 8)
searchBoxCorner.Parent = searchBox

local searchButton = Instance.new("TextButton", searchFrame)
searchButton.Size = UDim2.new(0.4, 0, 0, 30)
searchButton.Position = UDim2.new(0.3, 0, 0, 55)
searchButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
searchButton.Text = "🔍 بحث"
searchButton.TextColor3 = Color3.fromRGB(255, 255, 255)
searchButton.TextSize = 14
searchButton.Font = Enum.Font.GothamBold

local searchBtnCorner = Instance.new("UICorner")
searchBtnCorner.CornerRadius = UDim.new(0, 8)
searchBtnCorner.Parent = searchButton

-- معلومات اللاعب المحدد
local infoFrame = Instance.new("Frame", playerContent)
infoFrame.Size = UDim2.new(1, -10, 0, 150)
infoFrame.Position = UDim2.new(0, 5, 0, 110)
infoFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local infoFrameCorner = Instance.new("UICorner")
infoFrameCorner.CornerRadius = UDim.new(0, 12)
infoFrameCorner.Parent = infoFrame

local infoTitle = Instance.new("TextLabel", infoFrame)
infoTitle.Size = UDim2.new(1, -20, 0, 25)
infoTitle.Position = UDim2.new(0, 10, 0, 10)
infoTitle.BackgroundTransparency = 1
infoTitle.Text = "👤 معلومات اللاعب"
infoTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
infoTitle.TextSize = 16
infoTitle.Font = Enum.Font.GothamBold
infoTitle.TextXAlignment = Enum.TextXAlignment.Left

local playerInfoLabel = Instance.new("TextLabel", infoFrame)
playerInfoLabel.Size = UDim2.new(1, -20, 1, -45)
playerInfoLabel.Position = UDim2.new(0, 10, 0, 35)
playerInfoLabel.BackgroundTransparency = 1
playerInfoLabel.Text = "لم يتم تحديد لاعب"
playerInfoLabel.TextColor3 = Color3.fromRGB(200, 200, 255)
playerInfoLabel.TextSize = 12
playerInfoLabel.Font = Enum.Font.Gotham
playerInfoLabel.TextXAlignment = Enum.TextXAlignment.Left
playerInfoLabel.TextYAlignment = Enum.TextYAlignment.Top
playerInfoLabel.TextWrapped = true

-- أزرار الرقص
local danceFrame = Instance.new("Frame", playerContent)
danceFrame.Size = UDim2.new(1, -10, 0, 120)
danceFrame.Position = UDim2.new(0, 5, 0, 270)
danceFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local danceCorner = Instance.new("UICorner")
danceCorner.CornerRadius = UDim.new(0, 12)
danceCorner.Parent = danceFrame

local danceTitle = Instance.new("TextLabel", danceFrame)
danceTitle.Size = UDim2.new(1, -20, 0, 25)
danceTitle.Position = UDim2.new(0, 10, 0, 10)
danceTitle.BackgroundTransparency = 1
danceTitle.Text = "💃 تحكم الرقصات"
danceTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
danceTitle.TextSize = 16
danceTitle.Font = Enum.Font.GothamBold
danceTitle.TextXAlignment = Enum.TextXAlignment.Left

local bangButton = Instance.new("TextButton", danceFrame)
bangButton.Size = UDim2.new(0.45, 0, 0, 35)
bangButton.Position = UDim2.new(0.025, 0, 0, 45)
bangButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
bangButton.Text = "💥 بانج"
bangButton.TextColor3 = Color3.fromRGB(255, 255, 255)
bangButton.TextSize = 14
bangButton.Font = Enum.Font.GothamBold

local bangBtnCorner = Instance.new("UICorner")
bangBtnCorner.CornerRadius = UDim.new(0, 8)
bangBtnCorner.Parent = bangButton

local headBangButton = Instance.new("TextButton", danceFrame)
headBangButton.Size = UDim2.new(0.45, 0, 0, 35)
headBangButton.Position = UDim2.new(0.525, 0, 0, 45)
headBangButton.BackgroundColor3 = Color3.fromRGB(255, 50, 100)
headBangButton.Text = "💢 هيدبانج"
headBangButton.TextColor3 = Color3.fromRGB(255, 255, 255)
headBangButton.TextSize = 14
headBangButton.Font = Enum.Font.GothamBold

local headBangBtnCorner = Instance.new("UICorner")
headBangBtnCorner.CornerRadius = UDim.new(0, 8)
headBangBtnCorner.Parent = headBangButton

local stopDanceButton = Instance.new("TextButton", danceFrame)
stopDanceButton.Size = UDim2.new(0.45, 0, 0, 35)
stopDanceButton.Position = UDim2.new(0.275, 0, 0, 90)
stopDanceButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
stopDanceButton.Text = "⏹ إيقاف"
stopDanceButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopDanceButton.TextSize = 14
stopDanceButton.Font = Enum.Font.GothamBold

local stopDanceCorner = Instance.new("UICorner")
stopDanceCorner.CornerRadius = UDim.new(0, 8)
stopDanceCorner.Parent = stopDanceButton

-- ===========================================
-- علامة تبويب الأوامر (مع السرعة الفائقة)
-- ===========================================
local commandsContent = tabs.commands

local commandInputFrame = Instance.new("Frame", commandsContent)
commandInputFrame.Size = UDim2.new(1, -10, 0, 90)
commandInputFrame.Position = UDim2.new(0, 5, 0, 10)
commandInputFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 12)
inputCorner.Parent = commandInputFrame

local commandBox = Instance.new("TextBox", commandInputFrame)
commandBox.Size = UDim2.new(1, -20, 0, 40)
commandBox.Position = UDim2.new(0, 10, 0, 10)
commandBox.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
commandBox.TextColor3 = Color3.fromRGB(255, 255, 255)
commandBox.PlaceholderText = "اكتب الأمر (ابدأ بـ ;)"
commandBox.Text = ""
commandBox.TextSize = 14
commandBox.Font = Enum.Font.Gotham
commandBox.ClearTextOnFocus = false

local boxCorner1 = Instance.new("UICorner")
boxCorner1.CornerRadius = UDim.new(0, 8)
boxCorner1.Parent = commandBox

-- حقل السرعة (إزالة لاستبداله بالسرعة الفائقة)
local fastSpeedFrame = Instance.new("Frame", commandsContent)
fastSpeedFrame.Size = UDim2.new(1, -10, 0, 50)
fastSpeedFrame.Position = UDim2.new(0, 5, 0, 110)
fastSpeedFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local fastSpeedCorner = Instance.new("UICorner")
fastSpeedCorner.CornerRadius = UDim.new(0, 12)
fastSpeedCorner.Parent = fastSpeedFrame

local fastSpeedLabel = Instance.new("TextLabel", fastSpeedFrame)
fastSpeedLabel.Size = UDim2.new(1, -20, 0, 25)
fastSpeedLabel.Position = UDim2.new(0, 10, 0, 12)
fastSpeedLabel.BackgroundTransparency = 1
fastSpeedLabel.Text = "⚡ سبام فائق السرعة"
fastSpeedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
fastSpeedLabel.TextSize = 12
fastSpeedLabel.Font = Enum.Font.GothamBold
fastSpeedLabel.TextXAlignment = Enum.TextXAlignment.Left

-- أزرار التحكم
local buttonsFrame = Instance.new("Frame", commandsContent)
buttonsFrame.Size = UDim2.new(1, -10, 0, 100)
buttonsFrame.Position = UDim2.new(0, 5, 0, 170)
buttonsFrame.BackgroundTransparency = 1

local executeButton = Instance.new("TextButton", buttonsFrame)
executeButton.Size = UDim2.new(0.48, 0, 0, 40)
executeButton.Position = UDim2.new(0, 0, 0, 0)
executeButton.BackgroundColor3 = Color3.fromRGB(0, 180, 100)
executeButton.Text = "🚀 تنفيذ"
executeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
executeButton.TextSize = 14
executeButton.Font = Enum.Font.GothamBold

local execCorner = Instance.new("UICorner")
execCorner.CornerRadius = UDim.new(0, 8)
execCorner.Parent = executeButton

local startSpamButton = Instance.new("TextButton", buttonsFrame)
startSpamButton.Size = UDim2.new(0.48, 0, 0, 40)
startSpamButton.Position = UDim2.new(0.52, 0, 0, 0)
startSpamButton.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
startSpamButton.Text = "⚡ سبام"
startSpamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
startSpamButton.TextSize = 14
startSpamButton.Font = Enum.Font.GothamBold

local spamCorner = Instance.new("UICorner")
spamCorner.CornerRadius = UDim.new(0, 8)
spamCorner.Parent = startSpamButton

local stopSpamButton = Instance.new("TextButton", buttonsFrame)
stopSpamButton.Size = UDim2.new(0.48, 0, 0, 40)
stopSpamButton.Position = UDim2.new(0, 0, 0, 50)
stopSpamButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
stopSpamButton.Text = "⏹ إيقاف"
stopSpamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopSpamButton.TextSize = 14
stopSpamButton.Font = Enum.Font.GothamBold

local stopCorner = Instance.new("UICorner")
stopCorner.CornerRadius = UDim.new(0, 8)
stopCorner.Parent = stopSpamButton

local ultraSpamButton = Instance.new("TextButton", buttonsFrame)
ultraSpamButton.Size = UDim2.new(0.48, 0, 0, 40)
ultraSpamButton.Position = UDim2.new(0.52, 0, 0, 50)
ultraSpamButton.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
ultraSpamButton.Text = "🚀 سبام فائق"
ultraSpamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ultraSpamButton.TextSize = 13
ultraSpamButton.Font = Enum.Font.GothamBold

local ultraSpamCorner = Instance.new("UICorner")
ultraSpamCorner.CornerRadius = UDim.new(0, 8)
ultraSpamCorner.Parent = ultraSpamButton

-- منطقة سجل الأوامر
local historyFrame = Instance.new("Frame", commandsContent)
historyFrame.Size = UDim2.new(1, -10, 0, 150)
historyFrame.Position = UDim2.new(0, 5, 0, 280)
historyFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local historyCorner = Instance.new("UICorner")
historyCorner.CornerRadius = UDim.new(0, 12)
historyCorner.Parent = historyFrame

local historyLabel = Instance.new("TextLabel", historyFrame)
historyLabel.Size = UDim2.new(1, -20, 0, 25)
historyLabel.Position = UDim2.new(0, 10, 0, 10)
historyLabel.BackgroundTransparency = 1
historyLabel.Text = "📜 السجل"
historyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
historyLabel.TextSize = 14
historyLabel.Font = Enum.Font.GothamBold
historyLabel.TextXAlignment = Enum.TextXAlignment.Left

local historyScroll = Instance.new("ScrollingFrame", historyFrame)
historyScroll.Size = UDim2.new(1, -20, 1, -45)
historyScroll.Position = UDim2.new(0, 10, 0, 35)
historyScroll.BackgroundTransparency = 1
historyScroll.ScrollBarThickness = 4
historyScroll.CanvasSize = UDim2.new(0, 0, 0, 0)

-- ===========================================
-- علامة تبويب الشات
-- ===========================================
local chatContent = tabs.chat

-- حقل رسالة الشات
local chatInputFrame = Instance.new("Frame", chatContent)
chatInputFrame.Size = UDim2.new(1, -10, 0, 100)
chatInputFrame.Position = UDim2.new(0, 5, 0, 10)
chatInputFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local chatInputCorner = Instance.new("UICorner")
chatInputCorner.CornerRadius = UDim.new(0, 12)
chatInputCorner.Parent = chatInputFrame

local chatBox = Instance.new("TextBox", chatInputFrame)
chatBox.Size = UDim2.new(1, -20, 1, -20)
chatBox.Position = UDim2.new(0, 10, 0, 10)
chatBox.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
chatBox.TextColor3 = Color3.fromRGB(255, 255, 255)
chatBox.PlaceholderText = "اكتب رسالة الشات..."
chatBox.Text = ""
chatBox.TextSize = 14
chatBox.Font = Enum.Font.Gotham
chatBox.MultiLine = true
chatBox.TextWrapped = true
chatBox.ClearTextOnFocus = false

local chatBoxCorner = Instance.new("UICorner")
chatBoxCorner.CornerRadius = UDim.new(0, 8)
chatBoxCorner.Parent = chatBox

-- إعدادات الشات
local chatSettingsFrame = Instance.new("Frame", chatContent)
chatSettingsFrame.Size = UDim2.new(1, -10, 0, 70)
chatSettingsFrame.Position = UDim2.new(0, 5, 0, 120)
chatSettingsFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local settingsCorner = Instance.new("UICorner")
settingsCorner.CornerRadius = UDim.new(0, 12)
settingsCorner.Parent = chatSettingsFrame

local chatSpeedLabel = Instance.new("TextLabel", chatSettingsFrame)
chatSpeedLabel.Size = UDim2.new(0.4, 0, 0, 20)
chatSpeedLabel.Position = UDim2.new(0, 10, 0, 25)
chatSpeedLabel.BackgroundTransparency = 1
chatSpeedLabel.Text = "السرعة:"
chatSpeedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
chatSpeedLabel.TextSize = 12
chatSpeedLabel.Font = Enum.Font.GothamBold
chatSpeedLabel.TextXAlignment = Enum.TextXAlignment.Left

local chatSpeedBox = Instance.new("TextBox", chatSettingsFrame)
chatSpeedBox.Size = UDim2.new(0.5, 0, 0, 25)
chatSpeedBox.Position = UDim2.new(0.45, 0, 0, 22)
chatSpeedBox.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
chatSpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
chatSpeedBox.PlaceholderText = "1.0"
chatSpeedBox.Text = "1.0"
chatSpeedBox.TextSize = 12
chatSpeedBox.Font = Enum.Font.Gotham
chatSpeedBox.ClearTextOnFocus = false

local chatSpeedBoxCorner = Instance.new("UICorner")
chatSpeedBoxCorner.CornerRadius = UDim.new(0, 6)
chatSpeedBoxCorner.Parent = chatSpeedBox

-- أزرار الشات
local chatButtonsFrame = Instance.new("Frame", chatContent)
chatButtonsFrame.Size = UDim2.new(1, -10, 0, 100)
chatButtonsFrame.Position = UDim2.new(0, 5, 0, 200)
chatButtonsFrame.BackgroundTransparency = 1

local sendButton = Instance.new("TextButton", chatButtonsFrame)
sendButton.Size = UDim2.new(0.48, 0, 0, 40)
sendButton.Position = UDim2.new(0, 0, 0, 0)
sendButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
sendButton.Text = "📤 إرسال"
sendButton.TextColor3 = Color3.fromRGB(255, 255, 255)
sendButton.TextSize = 14
sendButton.Font = Enum.Font.GothamBold

local sendCorner = Instance.new("UICorner")
sendCorner.CornerRadius = UDim.new(0, 8)
sendCorner.Parent = sendButton

local startChatSpamButton = Instance.new("TextButton", chatButtonsFrame)
startChatSpamButton.Size = UDim2.new(0.48, 0, 0, 40)
startChatSpamButton.Position = UDim2.new(0.52, 0, 0, 0)
startChatSpamButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
startChatSpamButton.Text = "🔥 سبام"
startChatSpamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
startChatSpamButton.TextSize = 14
startChatSpamButton.Font = Enum.Font.GothamBold

local startChatCorner = Instance.new("UICorner")
startChatCorner.CornerRadius = UDim.new(0, 8)
startChatCorner.Parent = startChatSpamButton

local stopChatSpamButton = Instance.new("TextButton", chatButtonsFrame)
stopChatSpamButton.Size = UDim2.new(0.48, 0, 0, 40)
stopChatSpamButton.Position = UDim2.new(0, 0, 0, 50)
stopChatSpamButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
stopChatSpamButton.Text = "⏹ إيقاف"
stopChatSpamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopChatSpamButton.TextSize = 14
stopChatSpamButton.Font = Enum.Font.GothamBold

local stopChatCorner = Instance.new("UICorner")
stopChatCorner.CornerRadius = UDim.new(0, 8)
stopChatCorner.Parent = stopChatSpamButton

local saveMessageButton = Instance.new("TextButton", chatButtonsFrame)
saveMessageButton.Size = UDim2.new(0.48, 0, 0, 40)
saveMessageButton.Position = UDim2.new(0.52, 0, 0, 50)
saveMessageButton.BackgroundColor3 = Color3.fromRGB(100, 180, 100)
saveMessageButton.Text = "💾 حفظ"
saveMessageButton.TextColor3 = Color3.fromRGB(255, 255, 255)
saveMessageButton.TextSize = 14
saveMessageButton.Font = Enum.Font.GothamBold

local saveCorner = Instance.new("UICorner")
saveCorner.CornerRadius = UDim.new(0, 8)
saveCorner.Parent = saveMessageButton

-- سجل الشات
local chatHistoryFrame = Instance.new("Frame", chatContent)
chatHistoryFrame.Size = UDim2.new(1, -10, 0, 150)
chatHistoryFrame.Position = UDim2.new(0, 5, 0, 310)
chatHistoryFrame.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local chatHistoryCorner = Instance.new("UICorner")
chatHistoryCorner.CornerRadius = UDim.new(0, 12)
chatHistoryCorner.Parent = chatHistoryFrame

local chatHistoryLabel = Instance.new("TextLabel", chatHistoryFrame)
chatHistoryLabel.Size = UDim2.new(1, -20, 0, 25)
chatHistoryLabel.Position = UDim2.new(0, 10, 0, 10)
chatHistoryLabel.BackgroundTransparency = 1
chatHistoryLabel.Text = "📝 السجل"
chatHistoryLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
chatHistoryLabel.TextSize = 14
chatHistoryLabel.Font = Enum.Font.GothamBold
chatHistoryLabel.TextXAlignment = Enum.TextXAlignment.Left

local chatHistoryScroll = Instance.new("ScrollingFrame", chatHistoryFrame)
chatHistoryScroll.Size = UDim2.new(1, -20, 1, -45)
chatHistoryScroll.Position = UDim2.new(0, 10, 0, 35)
chatHistoryScroll.BackgroundTransparency = 1
chatHistoryScroll.ScrollBarThickness = 4
chatHistoryScroll.CanvasSize = UDim2.new(0, 0, 0, 0)

-- ===========================================
-- علامة تبويب الأدوات
-- ===========================================
local toolsContent = tabs.tools

-- بطاقة تنظيف الذاكرة
local memoryCard = Instance.new("Frame", toolsContent)
memoryCard.Size = UDim2.new(1, -10, 0, 70)
memoryCard.Position = UDim2.new(0, 5, 0, 10)
memoryCard.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local memoryCorner = Instance.new("UICorner")
memoryCorner.CornerRadius = UDim.new(0, 12)
memoryCorner.Parent = memoryCard

local memoryTitle = Instance.new("TextLabel", memoryCard)
memoryTitle.Size = UDim2.new(0.6, 0, 0, 30)
memoryTitle.Position = UDim2.new(0, 10, 0, 10)
memoryTitle.BackgroundTransparency = 1
memoryTitle.Text = "🧹 تنظيف"
memoryTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
memoryTitle.TextSize = 16
memoryTitle.Font = Enum.Font.GothamBold
memoryTitle.TextXAlignment = Enum.TextXAlignment.Left

local memoryButton = Instance.new("TextButton", memoryCard)
memoryButton.Size = UDim2.new(0, 80, 0, 30)
memoryButton.Position = UDim2.new(1, -90, 0, 20)
memoryButton.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
memoryButton.Text = "تنظيف"
memoryButton.TextColor3 = Color3.fromRGB(255, 255, 255)
memoryButton.TextSize = 12
memoryButton.Font = Enum.Font.GothamBold

local memoryButtonCorner = Instance.new("UICorner")
memoryButtonCorner.CornerRadius = UDim.new(0, 6)
memoryButtonCorner.Parent = memoryButton

-- بطاقة الإحصائيات
local statsCard = Instance.new("Frame", toolsContent)
statsCard.Size = UDim2.new(1, -10, 0, 130)
statsCard.Position = UDim2.new(0, 5, 0, 90)
statsCard.BackgroundColor3 = Color3.fromRGB(30, 35, 55)

local statsCorner = Instance.new("UICorner")
statsCorner.CornerRadius = UDim.new(0, 12)
statsCorner.Parent = statsCard

local statsTitle = Instance.new("TextLabel", statsCard)
statsTitle.Size = UDim2.new(1, -20, 0, 25)
statsTitle.Position = UDim2.new(0, 10, 0, 10)
statsTitle.BackgroundTransparency = 1
statsTitle.Text = "📊 إحصائيات"
statsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
statsTitle.TextSize = 16
statsTitle.Font = Enum.Font.GothamBold
statsTitle.TextXAlignment = Enum.TextXAlignment.Left

local statsText = Instance.new("TextLabel", statsCard)
statsText.Size = UDim2.new(1, -20, 0.7, -20)
statsText.Position = UDim2.new(0, 10, 0, 35)
statsText.BackgroundTransparency = 1
statsText.Text = "الأوامر: 0\nالرسائل: 0\nسبام الأوامر: ❌\nسبام الشات: ❌"
statsText.TextColor3 = Color3.fromRGB(200, 200, 255)
statsText.TextSize = 12
statsText.Font = Enum.Font.Gotham
statsText.TextXAlignment = Enum.TextXAlignment.Left
statsText.TextYAlignment = Enum.TextYAlignment.Top
statsText.TextWrapped = true

-- ===========================================
-- دوال التحديث
-- ===========================================
local commandsSent = 0
local messagesSent = 0

local function UpdateStats()
    local commandStatus = CommandSpamActive and "✅" or "❌"
    local chatStatus = ChatSpamActive and "✅" or "❌"
    
    statsText.Text = string.format("الأوامر: %d\nالرسائل: %d\nسبام الأوامر: %s\nسبام الشات: %s",
        commandsSent, messagesSent, commandStatus, chatStatus)
end

local function UpdateCommandHistory()
    for _, child in pairs(historyScroll:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end
    
    local yOffset = 0
    for i = 1, math.min(#commandHistory, 6) do
        local cmd = commandHistory[i]
        if cmd then
            local historyItem = Instance.new("TextButton")
            historyItem.Size = UDim2.new(1, 0, 0, 25)
            historyItem.Position = UDim2.new(0, 0, 0, yOffset)
            historyItem.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
            historyItem.TextColor3 = Color3.fromRGB(255, 255, 255)
            historyItem.Text = string.format("[%d] %s", i, cmd)
            historyItem.TextSize = 11
            historyItem.Font = Enum.Font.Gotham
            historyItem.TextXAlignment = Enum.TextXAlignment.Left
            historyItem.TextWrapped = true
            historyItem.AutoButtonColor = false
            
            local itemCorner = Instance.new("UICorner")
            itemCorner.CornerRadius = UDim.new(0, 4)
            itemCorner.Parent = historyItem
            
            historyItem.MouseButton1Click:Connect(function()
                commandBox.Text = cmd
                commandBox:CaptureFocus()
            end)
            
            historyItem.Parent = historyScroll
            yOffset = yOffset + 30
        end
    end
    
    historyScroll.CanvasSize = UDim2.new(0, 0, 0, yOffset)
end

local function UpdateChatHistory()
    for _, child in pairs(chatHistoryScroll:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end
    
    local yOffset = 0
    for i = 1, math.min(#chatHistory, 6) do
        local msg = chatHistory[i]
        if msg then
            local historyItem = Instance.new("TextButton")
            historyItem.Size = UDim2.new(1, 0, 0, 25)
            historyItem.Position = UDim2.new(0, 0, 0, yOffset)
            historyItem.BackgroundColor3 = Color3.fromRGB(40, 45, 70)
            historyItem.TextColor3 = Color3.fromRGB(255, 255, 255)
            historyItem.Text = string.format("[%d] %s", i, string.sub(msg, 1, 30))
            historyItem.TextSize = 11
            historyItem.Font = Enum.Font.Gotham
            historyItem.TextXAlignment = Enum.TextXAlignment.Left
            historyItem.TextWrapped = true
            historyItem.AutoButtonColor = false
            
            local itemCorner = Instance.new("UICorner")
            itemCorner.CornerRadius = UDim.new(0, 4)
            itemCorner.Parent = historyItem
            
            historyItem.MouseButton1Click:Connect(function()
                chatBox.Text = msg
                chatBox:CaptureFocus()
            end)
            
            historyItem.Parent = chatHistoryScroll
            yOffset = yOffset + 30
        end
    end
    
    chatHistoryScroll.CanvasSize = UDim2.new(0, 0, 0, yOffset)
end

-- ===========================================
-- دوال الأزرار
-- ===========================================

-- زر Anti Lag (مع 6 مستويات)
antiLagButton.MouseButton1Click:Connect(function()
    AntiLagLevel = (AntiLagLevel % 6) + 1
    
    if AntiLagLevel == 1 then
        AntiLag_Level1()
        antiLagLevel.Text = "المستوى: 1/6"
        antiLagButton.Text = "المستوى 2"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    elseif AntiLagLevel == 2 then
        AntiLag_Level2()
        antiLagLevel.Text = "المستوى: 2/6"
        antiLagButton.Text = "المستوى 3"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(0, 180, 100)
    elseif AntiLagLevel == 3 then
        AntiLag_Level3()
        antiLagLevel.Text = "المستوى: 3/6"
        antiLagButton.Text = "المستوى 4"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
    elseif AntiLagLevel == 4 then
        AntiLag_Level4()
        antiLagLevel.Text = "المستوى: 4/6"
        antiLagButton.Text = "المستوى 5"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    elseif AntiLagLevel == 5 then
        AntiLag_Level5()
        antiLagLevel.Text = "المستوى: 5/6"
        antiLagButton.Text = "المستوى 6"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
    else
        AntiLag_Level6()
        antiLagLevel.Text = "المستوى: 6/6 (MAX)"
        antiLagButton.Text = "إعادة تعيين"
        antiLagButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    end
end)

-- زر Anti AFK
antiAFKButton.MouseButton1Click:Connect(function()
    if not AntiAFK then
        EnableAntiAFK()
        antiAFKStatus.Text = "✅ نشط"
        antiAFKButton.Text = "إيقاف"
        antiAFKButton.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
    else
        DisableAntiAFK()
        antiAFKStatus.Text = "❌ متوقف"
        antiAFKButton.Text = "تفعيل"
        antiAFKButton.BackgroundColor3 = Color3.fromRGB(60, 180, 100)
    end
end)

-- زر Remove Chat
removeChatButton.MouseButton1Click:Connect(function()
    if not NoChat then
        NoChat = true
        pcall(function()
            TCS.ChatWindowConfiguration.Enabled = false
            TCS.BubbleChatConfiguration.Enabled = false
            for _, v in pairs(player.PlayerGui:GetChildren()) do
                if string.find(string.lower(v.Name), "chat") then
                    v:Destroy()
                end
            end
        end)
        removeChatStatus.Text = "✅ نشط"
        removeChatButton.Text = "إلغاء"
        removeChatButton.BackgroundColor3 = Color3.fromRGB(100, 150, 255)
        print("✅ Remove Chat: مفعل")
    else
        NoChat = false
        removeChatStatus.Text = "❌ متوقف"
        removeChatButton.Text = "تفعيل"
        removeChatButton.BackgroundColor3 = Color3.fromRGB(180, 80, 60)
        print("❌ Remove Chat: متوقف")
    end
end)

-- زر البحث عن لاعب
searchButton.MouseButton1Click:Connect(function()
    local searchTerm = searchBox.Text
    if searchTerm == "" then return end
    
    local foundPlayers = FindPlayer(searchTerm)
    if #foundPlayers == 0 then
        playerInfoLabel.Text = "❌ لم يتم العثور على لاعب"
        SelectedPlayer = nil
    elseif #foundPlayers == 1 then
        SelectedPlayer = foundPlayers[1]
        playerInfoLabel.Text = GetPlayerInfo(SelectedPlayer)
    else
        playerInfoLabel.Text = string.format("✅ تم العثور على %d لاعب:\n", #foundPlayers)
        for i, targetPlayer in ipairs(foundPlayers) do
            if i <= 3 then -- عرض أول 3 لاعبين فقط
                playerInfoLabel.Text = playerInfoLabel.Text .. string.format("\n%d. %s (@%s)", 
                    i, targetPlayer.DisplayName, targetPlayer.Name)
            end
        end
        if #foundPlayers > 3 then
            playerInfoLabel.Text = playerInfoLabel.Text .. string.format("\n\n...و %d لاعبين آخرين", #foundPlayers - 3)
        end
        playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n⭐ الرجاء إدخال اسم محدد أكثر"
    end
end)

-- زر بانج
bangButton.MouseButton1Click:Connect(function()
    if not SelectedPlayer then
        playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n❌ الرجاء تحديد لاعب أولاً"
        return
    end
    
    StopBang()
    StartBang(SelectedPlayer)
    playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n💥 تم تفعيل بانج!"
end)

-- زر هيدبانج
headBangButton.MouseButton1Click:Connect(function()
    if not SelectedPlayer then
        playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n❌ الرجاء تحديد لاعب أولاً"
        return
    end
    
    StopBang()
    StartHeadBang(SelectedPlayer)
    playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n💢 تم تفعيل هيدبانج!"
end)

-- زر إيقاف الرقصات
stopDanceButton.MouseButton1Click:Connect(function()
    StopBang()
    playerInfoLabel.Text = playerInfoLabel.Text .. "\n\n⏹ تم إيقاف الرقصات!"
end)

-- زر التنفيذ
executeButton.MouseButton1Click:Connect(function()
    local cmd = commandBox.Text
    if cmd ~= "" then
        if ExecuteCommand(cmd) then
            commandsSent = commandsSent + 1
            UpdateStats()
            UpdateCommandHistory()
        end
    end
end)

-- زر سبام الأوامر العادي
startSpamButton.MouseButton1Click:Connect(function()
    local cmd = commandBox.Text
    if cmd == "" then return end
    
    if CommandSpamActive then
        StopCommandSpam()
        startSpamButton.Text = "⚡ سبام"
        startSpamButton.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
    else
        local speed = 0.05 -- سرعة عالية
        StartCommandSpam(cmd, speed)
        startSpamButton.Text = "⏸️ إيقاف"
        startSpamButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    end
    UpdateStats()
end)

-- زر سبام الأوامر الفائق
ultraSpamButton.MouseButton1Click:Connect(function()
    local cmd = commandBox.Text
    if cmd == "" then return end
    
    if CommandSpamActive then
        StopCommandSpam()
        ultraSpamButton.Text = "🚀 سبام فائق"
        ultraSpamButton.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
    else
        StartUltraCommandSpam(cmd)
        ultraSpamButton.Text = "⏸️ إيقاف"
        ultraSpamButton.BackgroundColor3 = Color3.fromRGB(255, 50, 150)
    end
    UpdateStats()
end)

-- زر إيقاف سبام الأوامر
stopSpamButton.MouseButton1Click:Connect(function()
    StopCommandSpam()
    startSpamButton.Text = "⚡ سبام"
    startSpamButton.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
    ultraSpamButton.Text = "🚀 سبام فائق"
    ultraSpamButton.BackgroundColor3 = Color3.fromRGB(180, 0, 255)
    UpdateStats()
end)

-- زر إرسال الشات
sendButton.MouseButton1Click:Connect(function()
    local msg = chatBox.Text
    if msg ~= "" then
        if SendChatMessage(msg) then
            messagesSent = messagesSent + 1
            UpdateStats()
            UpdateChatHistory()
        end
    end
end)

-- زر سبام الشات
startChatSpamButton.MouseButton1Click:Connect(function()
    local msg = chatBox.Text
    local speedText = chatSpeedBox.Text
    
    if msg == "" then return end
    
    local speed = 1.0
    if speedText ~= "" then
        speed = tonumber(speedText) or 1.0
        if speed < 0.000000000001 then speed = 0.000000000001 end
        if speed > 10 then speed = 10 end
    end
    
    if ChatSpamActive then
        StopChatSpam()
        startChatSpamButton.Text = "🔥 سبام"
        startChatSpamButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    else
        StartChatSpam(msg, speed)
        startChatSpamButton.Text = "⏸️ إيقاف"
        startChatSpamButton.BackgroundColor3 = Color3.fromRGB(255, 80, 0)
    end
    UpdateStats()
end)

-- زر إيقاف سبام الشات
stopChatSpamButton.MouseButton1Click:Connect(function()
    StopChatSpam()
    startChatSpamButton.Text = "🔥 سبام"
    startChatSpamButton.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
    UpdateStats()
end)

-- زر حفظ الرسالة
saveMessageButton.MouseButton1Click:Connect(function()
    local msg = chatBox.Text
    if msg ~= "" then
        table.insert(savedMessages, 1, msg)
        if #savedMessages > 3 then
            table.remove(savedMessages, 4)
        end
        print("💾 تم حفظ الرسالة")
    end
end)

-- زر تنظيف الذاكرة
memoryButton.MouseButton1Click:Connect(function()
    CleanMemory()
end)

-- زر الإغلاق
closeButton.MouseButton1Click:Connect(function()
    StopCommandSpam()
    StopChatSpam()
    StopBang()
    if afkConnection then
        afkConnection:Disconnect()
    end
    mainGUI:Destroy()
    print("📴 تم إغلاق MrYesHackk Script")
end)

-- التحقق من السرعة
chatSpeedBox.FocusLost:Connect(function()
    local speedText = chatSpeedBox.Text
    if speedText ~= "" then
        local speed = tonumber(speedText)
        if speed then
            if speed < 0.000000000001 then
                chatSpeedBox.Text = "0.000000000001"
            elseif speed > 10 then
                chatSpeedBox.Text = "10"
            end
        else
            chatSpeedBox.Text = "1.0"
        end
    end
end)

-- أزرار علامات التبويب
mainTabBtn.MouseButton1Click:Connect(function() SwitchTab("main") end)
playerTabBtn.MouseButton1Click:Connect(function() 
    SwitchTab("player")
end)
commandsTabBtn.MouseButton1Click:Connect(function() 
    SwitchTab("commands")
    UpdateCommandHistory()
end)
chatTabBtn.MouseButton1Click:Connect(function() 
    SwitchTab("chat")
    UpdateChatHistory()
end)
toolsTabBtn.MouseButton1Click:Connect(function() 
    SwitchTab("tools")
    UpdateStats()
end)

-- اختصارات لوحة المفاتيح
local UIS = game:GetService("UserInputService")
UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F1 then
        mainFrame.Visible = not mainFrame.Visible
    end
end)

-- ===========================================
-- التهيئة
-- ===========================================
UpdateStats()

print("\n" .. string.rep("=", 60))
print("👑 MrYesHackk Script v3.0")
print("🔥 Ultimate Edition - Loaded Successfully")
print("⚡ Developer: MrYesHackk")
print("🎮 Features:")
print("  ✅ Anti Lag (6 مستويات متقدمة)")
print("  ✅ Anti AFK & Remove Chat")
print("  ✅ Command Spam (سرعة فائقة)")
print("  ✅ Chat Spam مع RemoteEvent")
print("  ✅ نظام تتبع اللاعبين")
print("  ✅ رقصات بانج وهيدبانج")
print("  ✅ سبام أوامر فائق السرعة")
print(string.rep("=", 60) .. "\n")

-- ===========================================
-- بداية الكود المضافة (نظام الرقص المتقدم)
-- ===========================================
-- Delta Script Bang & HeadBang Dancer
-- Made for Roblox
-- Anim ID: 5918726674

-- متغيرات إضافية لنظام الرقص المتقدم
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- إعادة تعريف المتغيرات الأساسية
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local playerGui = player:WaitForChild("PlayerGui")

-- إنشاء واجهة المستخدم المنفصلة
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BangDancerUI"
screenGui.ResetOnSpawn = false
screenGui.IgnoreGuiInset = true

-- الإطار الرئيسي القابل للسحب
local mainFrame2 = Instance.new("Frame")
mainFrame2.Name = "MainFrame"
mainFrame2.Size = UDim2.new(0, 380, 0, 320)
mainFrame2.Position = UDim2.new(0, 50, 0, 50)
mainFrame2.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
mainFrame2.BackgroundTransparency = 0
mainFrame2.BorderSizePixel = 0
mainFrame2.ClipsDescendants = true
mainFrame2.Active = true
mainFrame2.Selectable = true

-- إطار الظل لتحسين المظهر
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 8, 1, 8)
shadow.Position = UDim2.new(0, -4, 0, -4)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.7
shadow.BorderSizePixel = 0
shadow.ZIndex = 0
shadow.Parent = mainFrame2

-- تسمية العنوان القابلة للسحب
local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
titleBar.BorderSizePixel = 0
titleBar.Active = true
titleBar.Selectable = true

local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, -100, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "🎭 بانج + هيدبانج (اسحبني)"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 19
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Right

local closeButton2 = Instance.new("TextButton")
closeButton2.Name = "CloseButton"
closeButton2.Size = UDim2.new(0, 35, 0, 35)
closeButton2.Position = UDim2.new(1, -40, 0.5, -17.5)
closeButton2.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
closeButton2.Text = "✕"
closeButton2.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton2.TextSize = 18
closeButton2.Font = Enum.Font.GothamBold
closeButton2.AutoButtonColor = true

-- زر تصغير/تكبير
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 35, 0, 35)
toggleButton.Position = UDim2.new(1, -80, 0.5, -17.5)
toggleButton.BackgroundColor3 = Color3.fromRGB(80, 80, 120)
toggleButton.Text = "🗕"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.TextSize = 18
toggleButton.Font = Enum.Font.GothamBold
toggleButton.AutoButtonColor = true

-- منطقة البحث عن لاعب
local searchFrame = Instance.new("Frame")
searchFrame.Name = "SearchFrame"
searchFrame.Size = UDim2.new(1, -20, 0, 90)
searchFrame.Position = UDim2.new(0, 10, 0, 60)
searchFrame.BackgroundTransparency = 1

local searchLabel = Instance.new("TextLabel")
searchLabel.Name = "SearchLabel"
searchLabel.Size = UDim2.new(1, 0, 0, 25)
searchLabel.Position = UDim2.new(0, 0, 0, 0)
searchLabel.BackgroundTransparency = 1
searchLabel.Text = "🔍 ابحث عن لاعب (اكتب حرفين):"
searchLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
searchLabel.TextSize = 16
searchLabel.TextXAlignment = Enum.TextXAlignment.Right

local searchBox2 = Instance.new("TextBox")
searchBox2.Name = "SearchBox"
searchBox2.Size = UDim2.new(1, 0, 0, 40)
searchBox2.Position = UDim2.new(0, 0, 0, 30)
searchBox2.BackgroundColor3 = Color3.fromRGB(45, 45, 65)
searchBox2.TextColor3 = Color3.fromRGB(255, 255, 255)
searchBox2.PlaceholderText = "اسم اللاعب أو جزء منه..."
searchBox2.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
searchBox2.Text = ""
searchBox2.TextSize = 16
searchBox2.ClearTextOnFocus = false
searchBox2.BorderSizePixel = 1
searchBox2.BorderColor3 = Color3.fromRGB(70, 70, 100)

local resultsFrame = Instance.new("ScrollingFrame")
resultsFrame.Name = "ResultsFrame"
resultsFrame.Size = UDim2.new(1, 0, 0, 160)
resultsFrame.Position = UDim2.new(0, 0, 1, 5)
resultsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
resultsFrame.BorderSizePixel = 1
resultsFrame.BorderColor3 = Color3.fromRGB(60, 60, 85)
resultsFrame.Visible = false
resultsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
resultsFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
resultsFrame.ScrollBarThickness = 6
resultsFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 130)

-- إعدادات متقدمة
local settingsFrame = Instance.new("Frame")
settingsFrame.Name = "SettingsFrame"
settingsFrame.Size = UDim2.new(1, -20, 0, 80)
settingsFrame.Position = UDim2.new(0, 10, 0, 160)
settingsFrame.BackgroundTransparency = 1

local headbangDistanceLabel = Instance.new("TextLabel")
headbangDistanceLabel.Name = "HeadbangDistanceLabel"
headbangDistanceLabel.Size = UDim2.new(0.5, -5, 0, 25)
headbangDistanceLabel.Position = UDim2.new(0, 0, 0, 0)
headbangDistanceLabel.BackgroundTransparency = 1
headbangDistanceLabel.Text = "📏 مسافة هيدبانج:"
headbangDistanceLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
headbangDistanceLabel.TextSize = 14
headbangDistanceLabel.TextXAlignment = Enum.TextXAlignment.Right

local headbangDistanceSlider = Instance.new("TextBox")
headbangDistanceSlider.Name = "HeadbangDistanceSlider"
headbangDistanceSlider.Size = UDim2.new(0.5, -5, 0, 25)
headbangDistanceSlider.Position = UDim2.new(0.5, 5, 0, 0)
headbangDistanceSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
headbangDistanceSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
headbangDistanceSlider.Text = "0.2"
headbangDistanceSlider.TextSize = 14
headbangDistanceSlider.BorderSizePixel = 1
headbangDistanceSlider.BorderColor3 = Color3.fromRGB(70, 70, 100)

local headbangSpeedLabel = Instance.new("TextLabel")
headbangSpeedLabel.Name = "HeadbangSpeedLabel"
headbangSpeedLabel.Size = UDim2.new(0.5, -5, 0, 25)
headbangSpeedLabel.Position = UDim2.new(0, 0, 0, 30)
headbangSpeedLabel.BackgroundTransparency = 1
headbangSpeedLabel.Text = "⚡ سرعة حركة هيدبانج:"
headbangSpeedLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
headbangSpeedLabel.TextSize = 14
headbangSpeedLabel.TextXAlignment = Enum.TextXAlignment.Right

local headbangSpeedSlider = Instance.new("TextBox")
headbangSpeedSlider.Name = "HeadbangSpeedSlider"
headbangSpeedSlider.Size = UDim2.new(0.5, -5, 0, 25)
headbangSpeedSlider.Position = UDim2.new(0.5, 5, 0, 30)
headbangSpeedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
headbangSpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
headbangSpeedSlider.Text = "0.1"
headbangSpeedSlider.TextSize = 14
headbangSpeedSlider.BorderSizePixel = 1
headbangSpeedSlider.BorderColor3 = Color3.fromRGB(70, 70, 100)

-- الأزرار
local buttonsFrame2 = Instance.new("Frame")
buttonsFrame2.Name = "ButtonsFrame"
buttonsFrame2.Size = UDim2.new(1, -20, 0, 130)
buttonsFrame2.Position = UDim2.new(0, 10, 0, 250)
buttonsFrame2.BackgroundTransparency = 1

-- زر البانج
local bangButton2 = Instance.new("TextButton")
bangButton2.Name = "BangButton"
bangButton2.Size = UDim2.new(0.48, 0, 0, 50)
bangButton2.Position = UDim2.new(0, 0, 0, 0)
bangButton2.BackgroundColor3 = Color3.fromRGB(65, 105, 185)
bangButton2.Text = "🎭 بانج"
bangButton2.TextColor3 = Color3.fromRGB(255, 255, 255)
bangButton2.TextSize = 19
bangButton2.Font = Enum.Font.GothamBold
bangButton2.AutoButtonColor = true

-- زر الهيدبانج
local headbangButton2 = Instance.new("TextButton")
headbangButton2.Name = "HeadbangButton"
headbangButton2.Size = UDim2.new(0.48, 0, 0, 50)
headbangButton2.Position = UDim2.new(0.52, 0, 0, 0)
headbangButton2.BackgroundColor3 = Color3.fromRGB(185, 65, 105)
headbangButton2.Text = "👁️ هيدبانج"
headbangButton2.TextColor3 = Color3.fromRGB(255, 255, 255)
headbangButton2.TextSize = 19
headbangButton2.Font = Enum.Font.GothamBold
headbangButton2.AutoButtonColor = true

-- زر تفعيل الاثنين معًا
local dualButton = Instance.new("TextButton")
dualButton.Name = "DualButton"
dualButton.Size = UDim2.new(1, 0, 0, 40)
dualButton.Position = UDim2.new(0, 0, 0, 60)
dualButton.BackgroundColor3 = Color3.fromRGB(120, 85, 200)
dualButton.Text = "⚡ تفعيل الاثنين معًا"
dualButton.TextColor3 = Color3.fromRGB(255, 255, 255)
dualButton.TextSize = 17
dualButton.Font = Enum.Font.GothamBold
dualButton.AutoButtonColor = true

-- زر الإيقاف
local stopButton2 = Instance.new("TextButton")
stopButton2.Name = "StopButton"
stopButton2.Size = UDim2.new(1, 0, 0, 40)
stopButton2.Position = UDim2.new(0, 0, 0, 110)
stopButton2.BackgroundColor3 = Color3.fromRGB(55, 55, 75)
stopButton2.Text = "⏹️ إيقاف الكل"
stopButton2.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton2.TextSize = 17
stopButton2.Font = Enum.Font.GothamBold
stopButton2.AutoButtonColor = true

-- حالة التفعيل
local statusLabel = Instance.new("TextLabel")
statusLabel.Name = "StatusLabel"
statusLabel.Size = UDim2.new(1, -20, 0, 30)
statusLabel.Position = UDim2.new(0, 10, 1, -40)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "⚪ غير مفعل"
statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
statusLabel.TextSize = 15
statusLabel.TextXAlignment = Enum.TextXAlignment.Right

-- تجميع الواجهة
headbangSpeedSlider.Parent = settingsFrame
headbangSpeedLabel.Parent = settingsFrame
headbangDistanceSlider.Parent = settingsFrame
headbangDistanceLabel.Parent = settingsFrame
settingsFrame.Parent = mainFrame2

toggleButton.Parent = titleBar
closeButton2.Parent = titleBar
titleLabel.Parent = titleBar
titleBar.Parent = mainFrame2
stopButton2.Parent = buttonsFrame2
dualButton.Parent = buttonsFrame2
headbangButton2.Parent = buttonsFrame2
bangButton2.Parent = buttonsFrame2
buttonsFrame2.Parent = mainFrame2
statusLabel.Parent = mainFrame2
resultsFrame.Parent = searchFrame
searchBox2.Parent = searchFrame
searchLabel.Parent = searchFrame
searchFrame.Parent = mainFrame2
mainFrame2.Parent = screenGui
screenGui.Parent = playerGui

-- جعل النافذة قابلة للسحب من أي مكان فيها
local dragging = false
local dragInput, dragStart, startPos

local function updateInput(input)
	local delta = input.Position - dragStart
	local newX = startPos.X.Offset + delta.X
	local newY = startPos.Y.Offset + delta.Y
	
	-- التأكد من بقاء النافذة داخل الشاشة
	local viewportSize = workspace.CurrentCamera.ViewportSize
	newX = math.clamp(newX, 0, viewportSize.X - mainFrame2.AbsoluteSize.X)
	newY = math.clamp(newY, 0, viewportSize.Y - mainFrame2.AbsoluteSize.Y)
	
	mainFrame2.Position = UDim2.new(0, newX, 0, newY)
end

-- جعل الإطار كله قابل للسحب
mainFrame2.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = mainFrame2.Position
		
		-- تأثير مرئي عند السحب
		mainFrame2.BackgroundColor3 = Color3.fromRGB(25, 25, 40)
		titleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
		
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
				mainFrame2.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
				titleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
			end
		end)
	end
end)

mainFrame2.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input == dragInput then
		updateInput(input)
	end
end)

-- زر الإغلاق
closeButton2.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)

closeButton2.MouseEnter:Connect(function()
	closeButton2.BackgroundColor3 = Color3.fromRGB(220, 80, 80)
end)

closeButton2.MouseLeave:Connect(function()
	closeButton2.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
end)

-- زر التصغير/التكبير
local isMinimized = false
local originalSize = mainFrame2.Size
local originalPosition = mainFrame2.Position

toggleButton.MouseButton1Click:Connect(function()
	if isMinimized then
		-- تكبير
		mainFrame2.Size = originalSize
		searchFrame.Visible = true
		settingsFrame.Visible = true
		buttonsFrame2.Visible = true
		statusLabel.Visible = true
		toggleButton.Text = "🗕"
		isMinimized = false
	else
		-- تصغير
		originalSize = mainFrame2.Size
		originalPosition = mainFrame2.Position
		mainFrame2.Size = UDim2.new(0, 380, 0, 50)
		searchFrame.Visible = false
		settingsFrame.Visible = false
		buttonsFrame2.Visible = false
		statusLabel.Visible = false
		toggleButton.Text = "🗖"
		isMinimized = true
	end
end)

toggleButton.MouseEnter:Connect(function()
	toggleButton.BackgroundColor3 = Color3.fromRGB(100, 100, 140)
end)

toggleButton.MouseLeave:Connect(function()
	toggleButton.BackgroundColor3 = Color3.fromRGB(80, 80, 120)
end)

-- المتغيرات العامة للنظام المتقدم
local selectedPlayer2 = nil
local currentAnimation = nil
local currentHeadbangAnimation = nil
local animationSpeed2 = 3
local animationId2 = 5918726674
local isAnimating = false
local isHeadbanging2 = false
local animationConnection = nil
local headbangConnection = nil
local bangMode = "None" -- "Bang", "Headbang", "Dual", "None"

-- إعدادات هيدبانج للنظام المتقدم
local headbangOscillationDistance2 = 0.01 -- 1cm بالوحدات الروبلوكسية (تقريباً)
local headbangOscillationSpeed2 = 0.1 -- سرعة التذبذب
local headbangBaseDistance2 = 2.0 -- المسافة الأساسية من الوجه
local headbangOscillationTime2 = 0

-- تحديث حالة التفعيل للنظام المتقدم
local function updateStatus()
	if bangMode == "Bang" then
		statusLabel.Text = "🔴 بانج مفعل خلف: " .. (selectedPlayer2 and selectedPlayer2.Name or "غير محدد")
		statusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
	elseif bangMode == "Headbang" then
		statusLabel.Text = "👁️ هيدبانج مفعل أمام وجه: " .. (selectedPlayer2 and selectedPlayer2.Name or "غير محدد")
		statusLabel.TextColor3 = Color3.fromRGB(255, 150, 100)
	elseif bangMode == "Dual" then
		statusLabel.Text = "⚡ بانج + هيدبانج مفعلان معًا: " .. (selectedPlayer2 and selectedPlayer2.Name or "غير محدد")
		statusLabel.TextColor3 = Color3.fromRGB(200, 100, 255)
	else
		statusLabel.Text = "⚪ غير مفعل"
		statusLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
	end
end

-- إنشاء الأنيميشن للنظام المتقدم
local function loadAnimation2()
	local animation = Instance.new("Animation")
	animation.AnimationId = "rbxassetid://" .. animationId2
	return animation
end

-- البحث عن لاعب للنظام المتقدم
local function searchPlayers2(query)
	if #query < 2 then
		resultsFrame.Visible = false
		return
	end
	
	-- مسح النتائج السابقة
	for _, child in ipairs(resultsFrame:GetChildren()) do
		if child:IsA("TextButton") then
			child:Destroy()
		end
	end
	
	local players = Players:GetPlayers()
	local results = {}
	
	-- البحث عن اللاعبين الذين يطابقون الاستعلام
	for _, plr in ipairs(players) do
		if plr ~= player then
			local username = plr.Name
			local displayName = plr.DisplayName
			
			if username:lower():find(query:lower(), 1, true) or 
			   (displayName and displayName:lower():find(query:lower(), 1, true)) then
				table.insert(results, plr)
			end
		end
	end
	
	if #results > 0 then
		-- عرض النتائج
		for i, plr in ipairs(results) do
			local playerButton = Instance.new("TextButton")
			playerButton.Name = plr.Name
			playerButton.Size = UDim2.new(1, -10, 0, 40)
			playerButton.Position = UDim2.new(0, 5, 0, (i-1) * 45)
			playerButton.BackgroundColor3 = Color3.fromRGB(45, 45, 65)
			playerButton.Text = "👤 " .. plr.Name .. " (" .. plr.DisplayName .. ")"
			playerButton.TextColor3 = Color3.fromRGB(255, 255, 255)
			playerButton.TextSize = 14
			playerButton.AutoButtonColor = true
			
			playerButton.MouseEnter:Connect(function()
				playerButton.BackgroundColor3 = Color3.fromRGB(65, 65, 90)
			end)
			
			playerButton.MouseLeave:Connect(function()
				playerButton.BackgroundColor3 = Color3.fromRGB(45, 45, 65)
			end)
			
			playerButton.MouseButton1Click:Connect(function()
				selectedPlayer2 = plr
				searchBox2.Text = plr.Name
				resultsFrame.Visible = false
				updateStatus()
			end)
			
			playerButton.Parent = resultsFrame
		end
		
		resultsFrame.Visible = true
	else
		resultsFrame.Visible = false
	end
end

searchBox2:GetPropertyChangedSignal("Text"):Connect(function()
	searchPlayers2(searchBox2.Text)
end)

-- تحديث إعدادات هيدبانج من المنزلقات للنظام المتقدم
headbangDistanceSlider:GetPropertyChangedSignal("Text"):Connect(function()
	local value = tonumber(headbangDistanceSlider.Text)
	if value and value > 0 then
		headbangOscillationDistance2 = value
	end
end)

headbangSpeedSlider:GetPropertyChangedSignal("Text"):Connect(function()
	local value = tonumber(headbangSpeedSlider.Text)
	if value and value > 0 then
		headbangOscillationSpeed2 = value
	end
end)

-- إيقاف الكل للنظام المتقدم
local function stopAllAnimations2()
	-- إيقاف بانج
	if currentAnimation then
		if currentAnimation.AnimationTrack then
			currentAnimation.AnimationTrack:Stop()
		end
		currentAnimation = nil
	end
	
	-- إيقاف هيدبانج
	if currentHeadbangAnimation then
		if currentHeadbangAnimation.AnimationTrack then
			currentHeadbangAnimation.AnimationTrack:Stop()
		end
		currentHeadbangAnimation = nil
	end
	
	-- إيقاف الاتصالات
	if animationConnection then
		animationConnection:Disconnect()
		animationConnection = nil
	end
	
	if headbangConnection then
		headbangConnection:Disconnect()
		headbangConnection = nil
	end
	
	-- إعادة تعيين الأزرار
	bangButton2.BackgroundColor3 = Color3.fromRGB(65, 105, 185)
	headbangButton2.BackgroundColor3 = Color3.fromRGB(185, 65, 105)
	dualButton.BackgroundColor3 = Color3.fromRGB(120, 85, 200)
	
	isAnimating = false
	isHeadbanging2 = false
	bangMode = "None"
	headbangOscillationTime2 = 0
	
	updateStatus()
end

-- تشغيل الأنيميشن خلف اللاعب (مرتفع قليلاً) للنظام المتقدم
local function playBangAnimation2()
	if not selectedPlayer2 or not selectedPlayer2.Character then
		return
	end
	
	stopAllAnimations2()
	
	local targetCharacter = selectedPlayer2.Character
	local targetHRP = targetCharacter:FindFirstChild("HumanoidRootPart")
	local myHRP = character:FindFirstChild("HumanoidRootPart")
	local targetHumanoid = targetCharacter:FindFirstChild("Humanoid")
	
	if not targetHRP or not myHRP or not targetHumanoid then
		return
	end
	
	-- تحميل وتشغيل الأنيميشن
	local anim = loadAnimation2()
	local animationTrack = humanoid:LoadAnimation(anim)
	animationTrack:Play()
	animationTrack:AdjustSpeed(animationSpeed2)
	
	currentAnimation = {
		AnimationTrack = animationTrack,
		Target = targetCharacter
	}
	
	bangButton2.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
	isAnimating = true
	bangMode = "Bang"
	updateStatus()
	
	-- تحديث الموضع باستمرار ليبقى خلف الهدف ومرتفع قليلاً
	animationConnection = RunService.Heartbeat:Connect(function(deltaTime)
		if not isAnimating or not targetHRP or not myHRP or not selectedPlayer2.Character then
			return
		end
		
		local targetCFrame = targetHRP.CFrame
		local lookVector = targetCFrame.LookVector
		
		-- تحديد وضعية اللاعب
		local heightOffset = 0.5 -- ارتفاع إضافي فوق الأرض
		if targetHumanoid.Sit then
			heightOffset = -0.5
		end
		
		-- وضع اللاعب خلف الهدف مع ارتفاع إضافي
		local distanceBehind = 1.5
		local behindPosition = targetHRP.Position - (lookVector * distanceBehind)
		behindPosition = behindPosition + Vector3.new(0, heightOffset, 0) -- ارتفاع إضافي
		
		-- توجيه الشخصية نحو ظهر الهدف
		myHRP.CFrame = CFrame.new(behindPosition, behindPosition + lookVector)
		
		-- التأكد من أن الأنيميشن يعمل
		if not animationTrack.IsPlaying then
			animationTrack:Play()
			animationTrack:AdjustSpeed(animationSpeed2)
		end
	end)
end

-- تشغيل الهيدبانج أمام وجه اللاعب مع حركة تذبذب للنظام المتقدم
local function playHeadbangAnimation2()
	if not selectedPlayer2 or not selectedPlayer2.Character then
		return
	end
	
	stopAllAnimations2()
	
	local targetCharacter = selectedPlayer2.Character
	local targetHead = targetCharacter:FindFirstChild("Head")
	local targetHRP = targetCharacter:FindFirstChild("HumanoidRootPart")
	local myHRP = character:FindFirstChild("HumanoidRootPart")
	local targetHumanoid = targetCharacter:FindFirstChild("Humanoid")
	
	if not targetHead or not targetHRP or not myHRP or not targetHumanoid then
		return
	end
	
	-- تحميل وتشغيل الأنيميشن
	local anim = loadAnimation2()
	local animationTrack = humanoid:LoadAnimation(anim)
	animationTrack:Play()
	animationTrack:AdjustSpeed(animationSpeed2)
	
	currentHeadbangAnimation = {
		AnimationTrack = animationTrack,
		Target = targetCharacter
	}
	
	headbangButton2.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
	isHeadbanging2 = true
	bangMode = "Headbang"
	headbangOscillationTime2 = 0
	updateStatus()
	
	-- تحديث الموضع باستمرار مع حركة تذبذب
	headbangConnection = RunService.Heartbeat:Connect(function(deltaTime)
		if not isHeadbanging2 or not targetHead or not targetHRP or not myHRP or not selectedPlayer2.Character then
			return
		end
		
		-- تحديث وقت التذبذب
		headbangOscillationTime2 = headbangOscillationTime2 + (deltaTime * headbangOscillationSpeed2)
		
		-- حساب حركة التذبذب (1cm للخلف و1cm للأمام)
		local oscillation = math.sin(headbangOscillationTime2 * 2 * math.pi) * headbangOscillationDistance2
		
		local targetHeadPosition = targetHead.Position
		local targetCFrame = targetHRP.CFrame
		local lookVector = targetCFrame.LookVector
		
		-- تحديد وضعية اللاعب
		local heightOffset = 0
		if targetHumanoid.Sit then
			heightOffset = -0.3
		end
		
		-- المسافة من الوجه مع التذبذب
		local currentDistance = headbangBaseDistance2 + oscillation
		
		-- الموضع أمام الوجه مباشرة مع التذبذب
		local facePosition = targetHeadPosition + (lookVector * currentDistance)
		
		-- تعديل الارتفاع للنظر في عينيه
		facePosition = facePosition + Vector3.new(0, heightOffset + 0.2, 0)
		
		-- توجيه الشخصية نحو وجه اللاعب مباشرة
		local lookAtPosition = targetHeadPosition + Vector3.new(0, 0.5, 0) -- النظر إلى مستوى العيون
		
		-- وضع شخصيتنا أمام وجه اللاعب
		myHRP.CFrame = CFrame.new(facePosition, lookAtPosition)
		
		-- التأكد من أن الأنيميشن يعمل
		if not animationTrack.IsPlaying then
			animationTrack:Play()
			animationTrack:AdjustSpeed(animationSpeed2)
		end
	end)
end

-- تشغيل الاثنين معًا للنظام المتقدم
local function playDualAnimation2()
	if not selectedPlayer2 or not selectedPlayer2.Character then
		return
	end
	
	stopAllAnimations2()
	
	local targetCharacter = selectedPlayer2.Character
	local targetHead = targetCharacter:FindFirstChild("Head")
	local targetHRP = targetCharacter:FindFirstChild("HumanoidRootPart")
	local myHRP = character:FindFirstChild("HumanoidRootPart")
	local targetHumanoid = targetCharacter:FindFirstChild("Humanoid")
	
	if not targetHead or not targetHRP or not myHRP or not targetHumanoid then
		return
	end
	
	-- تحميل وتشغيل أنيميشن البانج
	local anim1 = loadAnimation2()
	local bangTrack = humanoid:LoadAnimation(anim1)
	bangTrack:Play()
	bangTrack:AdjustSpeed(animationSpeed2)
	
	currentAnimation = {
		AnimationTrack = bangTrack,
		Target = targetCharacter
	}
	
	-- تحميل وتشغيل أنيميشن الهيدبانج
	local anim2 = loadAnimation2()
	local headbangTrack = humanoid:LoadAnimation(anim2)
	headbangTrack:Play()
	headbangTrack:AdjustSpeed(animationSpeed2)
	
	currentHeadbangAnimation = {
		AnimationTrack = headbangTrack,
		Target = targetCharacter
	}
	
	bangButton2.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
	headbangButton2.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
	dualButton.BackgroundColor3 = Color3.fromRGB(255, 60, 120)
	
	isAnimating = true
	isHeadbanging2 = true
	bangMode = "Dual"
	headbangOscillationTime2 = 0
	updateStatus()
	
	-- تحديث الموضع ليكون خلف اللاعب ومرتفع قليلاً
	animationConnection = RunService.Heartbeat:Connect(function(deltaTime)
		if not isAnimating or not targetHRP or not myHRP or not selectedPlayer2.Character then
			return
		end
		
		local targetCFrame = targetHRP.CFrame
		local lookVector = targetCFrame.LookVector
		
		-- تحديد وضعية اللاعب
		local heightOffset = 0.5 -- ارتفاع إضافي
		if targetHumanoid.Sit then
			heightOffset = -0.5
		end
		
		-- وضع اللاعب خلف الهدف مع ارتفاع إضافي
		local distanceBehind = 1.5
		local behindPosition = targetHRP.Position - (lookVector * distanceBehind)
		behindPosition = behindPosition + Vector3.new(0, heightOffset, 0) -- ارتفاع إضافي
		
		-- توجيه الشخصية نحو ظهر الهدف
		myHRP.CFrame = CFrame.new(behindPosition, behindPosition + lookVector)
		
		-- التأكد من أن الأنيميشنات تعمل
		if not bangTrack.IsPlaying then
			bangTrack:Play()
			bangTrack:AdjustSpeed(animationSpeed2)
		end
		
		if not headbangTrack.IsPlaying then
			headbangTrack:Play()
			headbangTrack:AdjustSpeed(animationSpeed2)
		end
	end)
end

-- توصيل الأحداث مع تحسين التفاعل للنظام المتقدم
bangButton2.MouseButton1Click:Connect(function()
	if bangButton2.BackgroundColor3 == Color3.fromRGB(255, 60, 60) then
		-- إذا الزر أحمر (مفعل)، إيقافه
		stopAllAnimations2()
	else
		-- إذا الزر غير مفعل، تشغيله
		playBangAnimation2()
	end
end)

headbangButton2.MouseButton1Click:Connect(function()
	if headbangButton2.BackgroundColor3 == Color3.fromRGB(255, 60, 60) then
		-- إذا الزر أحمر (مفعل)، إيقافه
		stopAllAnimations2()
	else
		-- إذا الزر غير مفعل، تشغيله
		playHeadbangAnimation2()
	end
end)

dualButton.MouseButton1Click:Connect(function()
	if dualButton.BackgroundColor3 == Color3.fromRGB(255, 60, 120) then
		-- إذا الزر وردي (مفعل)، إيقافه
		stopAllAnimations2()
	else
		-- إذا الزر غير مفعل، تشغيله
		playDualAnimation2()
	end
end)

stopButton2.MouseButton1Click:Connect(function()
	stopAllAnimations2()
end)

-- تأثيرات عند المرور على الأزرار للنظام المتقدم
local function setupButtonHover(button, normalColor, hoverColor, activeColor)
	button.MouseEnter:Connect(function()
		if button.BackgroundColor3 ~= activeColor then
			button.BackgroundColor3 = hoverColor
		end
	end)
	
	button.MouseLeave:Connect(function()
		if button.BackgroundColor3 ~= activeColor then
			button.BackgroundColor3 = normalColor
		end
	end)
end

setupButtonHover(bangButton2, Color3.fromRGB(65, 105, 185), Color3.fromRGB(85, 125, 205), Color3.fromRGB(255, 60, 60))
setupButtonHover(headbangButton2, Color3.fromRGB(185, 65, 105), Color3.fromRGB(205, 85, 125), Color3.fromRGB(255, 60, 60))
setupButtonHover(dualButton, Color3.fromRGB(120, 85, 200), Color3.fromRGB(140, 105, 220), Color3.fromRGB(255, 60, 120))
setupButtonHover(stopButton2, Color3.fromRGB(55, 55, 75), Color3.fromRGB(75, 75, 95), Color3.fromRGB(55, 55, 75))

-- إغلاق قائمة النتائج عند النقر خارجها للنظام المتقدم
UserInputService.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		local mousePos = UserInputService:GetMouseLocation()
		local resultsAbsolutePos = resultsFrame.AbsolutePosition
		local resultsAbsoluteSize = resultsFrame.AbsoluteSize
		
		if resultsFrame.Visible and 
		   (mousePos.X < resultsAbsolutePos.X or 
			mousePos.X > resultsAbsolutePos.X + resultsAbsoluteSize.X or
			mousePos.Y < resultsAbsolutePos.Y or 
			mousePos.Y > resultsAbsolutePos.Y + resultsAbsoluteSize.Y) then
			
			local searchBoxPos = searchBox2.AbsolutePosition
			local searchBoxSize = searchBox2.AbsoluteSize
			
			if mousePos.X < searchBoxPos.X or 
			   mousePos.X > searchBoxPos.X + searchBoxSize.X or
			   mousePos.Y < searchBoxPos.Y or 
			   mousePos.Y > searchBoxPos.Y + searchBoxSize.Y then
				resultsFrame.Visible = false
			end
		end
	end
end)

-- إعادة التفعيل عند موت الشخصية أو تغييرها للنظام المتقدم
player.CharacterAdded:Connect(function(newChar)
	character = newChar
	humanoid = character:WaitForChild("Humanoid")
	
	-- إعادة التفعيل إذا كان النمط نشطًا
	if bangMode ~= "None" then
		task.wait(1) -- انتظار تحميل الشخصية
		
		if bangMode == "Bang" then
			playBangAnimation2()
		elseif bangMode == "Headbang" then
			playHeadbangAnimation2()
		elseif bangMode == "Dual" then
			playDualAnimation2()
		end
	end
end)

-- تنظيف عند تدمير السكريبت للنظام المتقدم
screenGui.Destroying:Connect(function()
	stopAllAnimations2()
end)

-- رسالة بدء التشغيل للنظام المتقدم
print("🎭 تم تحميل نظام رقصة بانج + هيدبانج المتطور!")
print("📌 الميزات الجديدة:")
print("   - واجهة منفصلة قابلة للسحب")
print("   - بانج مرتفع قليلاً فوق الأرض")
print("   - هيدبانج مع حركة تذبذب 1cm للأمام والخلف")
print("   - إمكانية تفعيل الاثنين معًا في نفس الوقت")
print("   - إعدادات قابلة للتعديل لحركة الهيدبانج")
print("   - زر جديد لتفعيل الاثنين معًا")
print("   - واجهة قابلة للسحب من أي مكان")
print("   - زر إيقاف الكل")

-- ===========================================
-- نهاية الكود المضافة
-- ===========================================
