-- ==========================================================
-- NERO FE v20.0 [PREMIUM OMNI EDITION]
-- Lógica Avançada, Adaptador de Jogos Dinâmico e Interface Premium
-- Criadores: Dark & DemonFrota
-- ==========================================================
getgenv().NERO_FE_LOADED = nil 
task.wait(0.1)
getgenv().NERO_FE_LOADED = true

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local TS = game:GetService("TweenService")
local StarterGui = game:GetService("StarterGui")
local Lighting = game:GetService("Lighting")
local LP = Players.LocalPlayer
local Camera = workspace.CurrentCamera

local NERO = {
    ESP = false, Aimbot = false, Fly = false, Noclip = false,
    Kick = false, Shaders = false, Emotes = false, Antifling = false,
    Imortal = false, Tornado = false, Invisible = false,
    Backpack = false, Beijo = false, FlipCar = false, Launch = false,
    Speed = false, SpeedVal = 50, Jump = false,
    FOV = 150, TornadoRaio = 50, TornadoForca = 1000,
    TargetName = "", SavedPos = nil,
    Rainbow = false, Trail = false, DiscoSky = false, Spinbot = false, MoonGravity = false,
    Connections = {},
    
    -- [PREMIUM OMNI MODES]
    QuantumTrigger = false, AetherMagnet = false, ChronosOverdrive = false,
    MatrixDesync = false, GameSpecificMod1 = false, GameSpecificMod2 = false,
    
    -- [FUNÇÕES PREMIUM]
    Blackhole = false, Midas = false, FlingAura = false,
    TimeStop = false, Hitbox = false, UltraInstinct = false, FakeLag = false,
    
    -- [NOVAS FUNÇÕES VISÍVEIS FE]
    FlingAll = false, GlitchWalk = false, ChatSpam = false
}

local C = {
    bg = Color3.fromRGB(0, 0, 0),         
    surface = Color3.fromRGB(15, 15, 15),    
    tabBg = Color3.fromRGB(10, 10, 10),      
    primary = Color3.fromRGB(255, 100, 0),  
    text = Color3.fromRGB(255, 255, 255),
    subtext = Color3.fromRGB(160, 160, 170),
    premium = Color3.fromRGB(255, 185, 70), 
    premiumBg = Color3.fromRGB(20, 10, 0)
}

local GameName = "Universal Sandbox"
local placeId = game.PlaceId

if placeId == 2753915549 or placeId == 4442272186 then
    GameName = "Blox Fruits"
elseif placeId == 4924922222 then
    GameName = "Brookhaven RP"
elseif placeId == 13772394625 then
    GameName = "Blade Ball"
elseif placeId == 6872265039 then
    GameName = "BedWars"
else
    pcall(function()
        local marketInfo = game:GetService("MarketplaceService"):GetProductInfo(placeId)
        if marketInfo and marketInfo.Name then GameName = marketInfo.Name end
    end)
end

local function Notify(texto)
    pcall(function()
        StarterGui:SetCore("SendNotification", { Title = "NERO FE", Text = texto, Duration = 3, Icon = "rbxassetid://10859948480" })
    end)
end

local function PremiumNotify(title, desc)
    task.spawn(function()
        if not Gui:FindFirstChild("PremiumNotifyContainer") then
            local pContainer = Instance.new("Frame", Gui)
            pContainer.Name = "PremiumNotifyContainer"
            pContainer.Size = UDim2.new(0, 280, 1, 0)
            pContainer.Position = UDim2.new(1, -290, 0, 0)
            pContainer.BackgroundTransparency = 1
        end
        local pNotify = Instance.new("Frame")
        pNotify.Size = UDim2.new(0, 260, 0, 70)
        pNotify.Position = UDim2.new(1, 30, 0.8, 0)
        pNotify.BackgroundColor3 = Color3.fromRGB(18, 17, 20)
        pNotify.Parent = Gui
        Instance.new("UICorner", pNotify).CornerRadius = UDim.new(0, 12)
        local stroke = Instance.new("UIStroke", pNotify)
        stroke.Color = C.premium; stroke.Thickness = 1.5
        local gradient = Instance.new("UIGradient", pNotify)
        gradient.Color = ColorSequence.new({ ColorSequenceKeypoint.new(0, C.primary), ColorSequenceKeypoint.new(1, C.premium) })
        
        local tLbl = Instance.new("TextLabel", pNotify)
        tLbl.Size = UDim2.new(1, -20, 0, 25); tLbl.Position = UDim2.new(0, 12, 0, 6)
        tLbl.BackgroundTransparency = 1; tLbl.Text = "👑 " .. title:upper()
        tLbl.TextColor3 = Color3.fromRGB(255, 255, 255); tLbl.Font = Enum.Font.GothamBold
        tLbl.TextSize = 13; tLbl.TextXAlignment = Enum.TextXAlignment.Left
        
        local dLbl = Instance.new("TextLabel", pNotify)
        dLbl.Size = UDim2.new(1, -24, 0, 35); dLbl.Position = UDim2.new(0, 12, 0, 28)
        dLbl.BackgroundTransparency = 1; dLbl.Text = desc
        dLbl.TextColor3 = Color3.fromRGB(200, 200, 210); dLbl.Font = Enum.Font.Gotham
        dLbl.TextSize = 10; dLbl.TextXAlignment = Enum.TextXAlignment.Left; dLbl.TextWrapped = true
        
        local sound = Instance.new("Sound", game.Workspace)
        sound.SoundId = "rbxassetid://138084657"; sound.Volume = 0.6; sound:Play()
        game.Debris:AddItem(sound, 2)
        
        TS:Create(pNotify, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Position = UDim2.new(1, -280, 0.8, 0)}):Play()
        task.wait(3.5)
        TS:Create(pNotify, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Position = UDim2.new(1, 30, 0.8, 0)}):Play()
        task.wait(0.3)
        pNotify:Destroy()
    end)
end

-- ==================== CHUVA DE CONFETES ====================
local function DispararConfetes(alvoGui)
    task.spawn(function()
        local confGui = Instance.new("ScreenGui")
        confGui.Name = "NeroConfetti"
        confGui.Parent = alvoGui.Parent
        
        local cores = {
            Color3.fromRGB(255, 126, 95), Color3.fromRGB(255, 185, 70), 
            Color3.fromRGB(255, 255, 255), Color3.fromRGB(150, 50, 255), 
            Color3.fromRGB(50, 255, 150)
        }
        
        for i = 1, 120 do
            local confete = Instance.new("Frame")
            confete.Size = UDim2.new(0, math.random(6, 12), 0, math.random(10, 20))
            confete.Position = UDim2.new(math.random(0, 100) / 100, 0, -0.1, 0)
            confete.BackgroundColor3 = cores[math.random(1, #cores)]
            confete.Rotation = math.random(0, 360)
            confete.BorderSizePixel = 0
            confete.Parent = confGui
            
            local tempo = math.random(25, 45) / 10
            local rotacaoFinal = confete.Rotation + math.random(-720, 720)
            
            local tween = TS:Create(confete, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {
                Position = UDim2.new(confete.Position.X.Scale, math.random(-50, 50), 1.2, 0),
                Rotation = rotacaoFinal
            })
            tween:Play()
            tween.Completed:Connect(function() confete:Destroy() end)
            task.wait(0.01)
        end
        game.Debris:AddItem(confGui, 6)
    end)
end

local function getSafeParent()
    if gethui then return gethui() end
    local success, coreGui = pcall(function() return game:GetService("CoreGui") end)
    if success and coreGui then return coreGui end
    return LP:WaitForChild("PlayerGui")
end

local targetParent = getSafeParent()
local oldGui = targetParent:FindFirstChild("NERO_HTML")
if oldGui then oldGui:Destroy() end

local Gui = Instance.new("ScreenGui")
Gui.Name = "NERO_HTML"
Gui.ResetOnSpawn = false
Gui.Parent = targetParent
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local Main = Instance.new("Frame")
Main.Size = UDim2.new(0, 330, 0, 465)
Main.Position = UDim2.new(0.5, -165, 0.5, -232)
Main.BackgroundColor3 = C.bg
Main.BorderSizePixel = 0
Main.Parent = Gui
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 35)

local BorderGlow = Instance.new("UIStroke", Main)
BorderGlow.Color = C.primary; BorderGlow.Thickness = 2
BorderGlow.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
local TweenInfoPulse = TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
TS:Create(BorderGlow, TweenInfoPulse, {Transparency = 0.8}):Play()

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -40, 0, 15)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Text = "X"
CloseBtn.TextColor3 = C.subtext
CloseBtn.TextSize = 18; CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.ZIndex = 10
CloseBtn.Parent = Main

-- ==================== CARROSSEL DE ABAS ATUALIZADO (12 ABAS) ====================
local TabFrame = Instance.new("ScrollingFrame")
TabFrame.Size = UDim2.new(1, -40, 0, 36)
TabFrame.Position = UDim2.new(0, 20, 0, 20)
TabFrame.BackgroundTransparency = 1
TabFrame.ScrollBarThickness = 0
TabFrame.ScrollingDirection = Enum.ScrollingDirection.X
TabFrame.Parent = Main

local tabs = {}
local tabNames = {"PLAYERS", "TROLLING", "TP", "VISUAIS", "CRÉDITOS", "CONFIG", "🌌 OMNI", "🔥 CAOS", "🧠 LÓGICA", "🎭 ILUSÃO", "💣 EXTREMO", "🗣️ SOCIAL"}
local tabWidths = {60, 65, 30, 55, 65, 55, 60, 60, 60, 60, 75, 75}
local tabContainers = {}

local currentX = 0
for i, name in ipairs(tabNames) do
    local w = tabWidths[i]
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, w, 0, 34)
    btn.Position = UDim2.new(0, currentX, 0, 0)
    currentX = currentX + w + 5
    btn.BackgroundColor3 = i == 1 and C.primary or C.tabBg
    btn.Text = name
    btn.TextColor3 = i == 1 and C.text or (name == "🌌 OMNI" and C.premium or C.primary)
    btn.TextSize = 8; btn.Font = Enum.Font.GothamBold; btn.Parent = TabFrame
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)

    if i == 1 then
        local bs = Instance.new("UIStroke", btn); bs.Color = C.primary; bs.Transparency = 0.6
    elseif name == "🌌 OMNI" then
        local bs = Instance.new("UIStroke", btn); bs.Color = C.premium; bs.Transparency = 0.4
    end

    local cont = Instance.new("ScrollingFrame")
    cont.Size = UDim2.new(1, -40, 0, 370)
    cont.Position = UDim2.new(0, 20, 0, 75)
    cont.BackgroundTransparency = 1
    cont.ScrollBarThickness = 2
    cont.CanvasSize = UDim2.new(0, 0, 0, 650)
    cont.Visible = i == 1; cont.Parent = Main
    tabContainers[i] = cont
    table.insert(tabs, btn)
end
TabFrame.CanvasSize = UDim2.new(0, currentX, 0, 0)

for i, btn in ipairs(tabs) do
    btn.MouseButton1Click:Connect(function()
        for j, b in ipairs(tabs) do
            local active = (j == i)
            if tabNames[j] == "🌌 OMNI" then
                b.BackgroundColor3 = active and C.premium or C.tabBg
                b.TextColor3 = active and Color3.fromRGB(0,0,0) or C.premium
            else
                b.BackgroundColor3 = active and C.primary or C.tabBg
                b.TextColor3 = active and C.text or C.primary
            end
            tabContainers[j].Visible = active
            local stroke = b:FindFirstChildOfClass("UIStroke")
            if stroke then stroke:Destroy() end
            if active then
                local bs = Instance.new("UIStroke", b)
                bs.Color = tabNames[j] == "🌌 OMNI" and C.premium or C.primary; bs.Transparency = 0.5
            end
        end
    end)
end

local function createToggle(name, parent, y)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 44); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.surface; frame.BorderSizePixel = 0; frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 20)
    
    local stroke = Instance.new("UIStroke", frame); stroke.Color = C.primary; stroke.Transparency = 0.7

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 180, 1, 0); label.Position = UDim2.new(0, 15, 0, 0)
    label.BackgroundTransparency = 1; label.Text = name; label.TextColor3 = C.text
    label.TextSize = 12; label.Font = Enum.Font.GothamMedium; label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame

    local toggleBg = Instance.new("Frame")
    toggleBg.Size = UDim2.new(0, 44, 0, 24); toggleBg.Position = UDim2.new(1, -55, 0, 10)
    toggleBg.BackgroundColor3 = C.tabBg; toggleBg.Parent = frame
    Instance.new("UICorner", toggleBg).CornerRadius = UDim.new(0, 12)

    local toggleKnob = Instance.new("Frame")
    toggleKnob.Size = UDim2.new(0, 20, 0, 20); toggleKnob.Position = UDim2.new(0, 2, 0, 2)
    toggleKnob.BackgroundColor3 = C.text; toggleKnob.Parent = toggleBg
    Instance.new("UICorner", toggleKnob).CornerRadius = UDim.new(0, 10)

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(1, 0, 1, 0); toggleBtn.BackgroundTransparency = 1; toggleBtn.Text = ""; toggleBtn.Parent = frame

    return toggleBg, toggleKnob, toggleBtn
end

local function createPremiumToggle(name, desc, parent, y)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 50); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.premiumBg; frame.BorderSizePixel = 0; frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 16)
    
    local stroke = Instance.new("UIStroke", frame); stroke.Color = C.premium; stroke.Thickness = 1; stroke.Transparency = 0.5

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 180, 0, 24); label.Position = UDim2.new(0, 15, 0, 4)
    label.BackgroundTransparency = 1; label.Text = name; label.TextColor3 = C.premium
    label.TextSize = 12; label.Font = Enum.Font.GothamBold; label.TextXAlignment = Enum.TextXAlignment.Left; label.Parent = frame

    local sublabel = Instance.new("TextLabel")
    sublabel.Size = UDim2.new(0, 180, 0, 16); sublabel.Position = UDim2.new(0, 15, 0, 24)
    sublabel.BackgroundTransparency = 1; sublabel.Text = desc; sublabel.TextColor3 = Color3.fromRGB(150, 150, 150)
    sublabel.TextSize = 9; sublabel.Font = Enum.Font.Gotham; sublabel.TextXAlignment = Enum.TextXAlignment.Left; sublabel.Parent = frame

    local toggleBg = Instance.new("Frame")
    toggleBg.Size = UDim2.new(0, 44, 0, 24); toggleBg.Position = UDim2.new(1, -55, 0, 13)
    toggleBg.BackgroundColor3 = Color3.fromRGB(35, 30, 25); toggleBg.Parent = frame
    Instance.new("UICorner", toggleBg).CornerRadius = UDim.new(0, 12)

    local toggleKnob = Instance.new("Frame")
    toggleKnob.Size = UDim2.new(0, 20, 0, 20); toggleKnob.Position = UDim2.new(0, 2, 0, 2)
    toggleKnob.BackgroundColor3 = Color3.fromRGB(220, 220, 220); toggleKnob.Parent = toggleBg
    Instance.new("UICorner", toggleKnob).CornerRadius = UDim.new(0, 10)

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(1, 0, 1, 0); toggleBtn.BackgroundTransparency = 1; toggleBtn.Text = ""; toggleBtn.Parent = frame

    return toggleBg, toggleKnob, toggleBtn
end

local function updateToggle(bg, knob, on, isPremium)
    local activeColor = isPremium and C.premium or C.primary
    local inactiveColor = isPremium and Color3.fromRGB(35, 30, 25) or C.tabBg
    if on then
        TS:Create(knob, TweenInfo.new(0.15), {Position = UDim2.new(1, -22, 0, 2)}):Play()
        TS:Create(bg, TweenInfo.new(0.15), {BackgroundColor3 = activeColor}):Play()
    else
        TS:Create(knob, TweenInfo.new(0.15), {Position = UDim2.new(0, 2, 0, 2)}):Play()
        TS:Create(bg, TweenInfo.new(0.15), {BackgroundColor3 = inactiveColor}):Play()
    end
end

local function findTarget()
    local name = NERO.TargetName:lower()
    if name == "" then return nil end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP and p.Character and p.Character:FindFirstChild("Head") and p.Name:lower():find(name) then
            return p.Character
        end
    end
    return nil
end

-- ==================== ABA 1: PLAYERS ====================
local ETog, EKnob, EBtn = createToggle("ESP", tabContainers[1], 0)
local ATog, AKnob, ABtn = createToggle("Aimbot", tabContainers[1], 48)
local FTog, FKnob, FBtn = createToggle("Fly (Necessita Recarregar)", tabContainers[1], 96)
local NTog, NKnob, NBtn = createToggle("Noclip", tabContainers[1], 144)
local SpTog, SpKnob, SBtn = createToggle("Speed Mod", tabContainers[1], 192)
local JpTog, JpKnob, JpBtn = createToggle("Jump Infinito", tabContainers[1], 240)
local ImTog, ImKnob, ImBtn = createToggle("Imortalidade", tabContainers[1], 288)

-- ==================== ABA 2: TROLLING ====================
local ToTog, ToKnob, ToBtn = createToggle("Tornado Orbit", tabContainers[2], 0)
local InvTog, InvKnob, InvBtn = createToggle("Invisível (Client)", tabContainers[2], 48)
local BpTog, BpKnob, BpBtn = createToggle("Backpack (Montar)", tabContainers[2], 96)
local BjTog, BjKnob, BjBtn = createToggle("Beijo / Kiss", tabContainers[2], 144)
local LauTog, LauKnob, LauBtn = createToggle("Launch Fling (E)", tabContainers[2], 192)

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, 0, 0, 44); TargetInput.Position = UDim2.new(0, 0, 0, 240)
TargetInput.BackgroundColor3 = C.surface; TargetInput.TextColor3 = C.text
TargetInput.TextSize = 12; TargetInput.Font = Enum.Font.Gotham; TargetInput.PlaceholderText = "Nome do Alvo para Troll..."
TargetInput.Text = ""; TargetInput.Parent = tabContainers[2]
Instance.new("UICorner", TargetInput).CornerRadius = UDim.new(0, 20)
local tis = Instance.new("UIStroke", TargetInput)
tis.Color = C.primary; tis.Transparency = 0.7
TargetInput.FocusLost:Connect(function() NERO.TargetName = TargetInput.Text; Notify("Alvo focado: " .. TargetInput.Text) end)

-- ==================== ABA 3: TP (TELEPORTE) ====================
local function createTabButton(name, parent, y, clickFunc)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 44); btn.Position = UDim2.new(0, 0, 0, y)
    btn.BackgroundColor3 = C.surface; btn.Text = name; btn.TextColor3 = C.text
    btn.TextSize = 12; btn.Font = Enum.Font.GothamMedium; btn.Parent = parent
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 20)
    local s = Instance.new("UIStroke", btn); s.Color = C.primary; s.Transparency = 0.7
    btn.MouseButton1Click:Connect(clickFunc)
    return btn
end

local spawnPos = Vector3.new(0, 50, 0)
pcall(function()
    local spawns = workspace:FindFirstChild("SpawnLocation") or workspace:FindFirstChild("Spawns")
    if spawns then spawnPos = spawns:GetPivot().Position + Vector3.new(0, 5, 0) end
end)

createTabButton("Teleportar para Torre (Spawn)", tabContainers[3], 0, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then 
        LP.Character.HumanoidRootPart.CFrame = CFrame.new(spawnPos); Notify("Teleportado para a Torre")
    end
end)

createTabButton("Teleportar para Ilha", tabContainers[3], 48, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then 
        LP.Character.HumanoidRootPart.CFrame = CFrame.new(0, 25, 0); Notify("Teleportado para a Ilha")
    end
end)

createTabButton("Salvar Local Atual", tabContainers[3], 96, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        NERO.SavedPos = LP.Character.HumanoidRootPart.CFrame; Notify("Local salvo com sucesso!")
    end
end)

createTabButton("Ir para Local Salvo", tabContainers[3], 144, function()
    if NERO.SavedPos and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.CFrame = NERO.SavedPos; Notify("Retornado ao local salvo")
    else
        Notify("Nenhum local foi salvo ainda!")
    end
end)

local TPPlayerFrame = Instance.new("Frame")
TPPlayerFrame.Size = UDim2.new(1, 0, 0, 44); TPPlayerFrame.Position = UDim2.new(0, 0, 0, 192)
TPPlayerFrame.BackgroundColor3 = C.surface; TPPlayerFrame.Parent = tabContainers[3]
Instance.new("UICorner", TPPlayerFrame).CornerRadius = UDim.new(0, 20)
local tpfs = Instance.new("UIStroke", TPPlayerFrame); tpfs.Color = C.primary; tpfs.Transparency = 0.7

local TPInputTab = Instance.new("TextBox")
TPInputTab.Size = UDim2.new(1, -90, 1, 0); TPInputTab.Position = UDim2.new(0, 15, 0, 0)
TPInputTab.BackgroundTransparency = 1; TPInputTab.TextColor3 = C.text
TPInputTab.TextSize = 12; TPInputTab.Font = Enum.Font.GothamMedium; TPInputTab.PlaceholderText = "Nome do player..."
TPInputTab.Text = ""; TPInputTab.TextXAlignment = Enum.TextXAlignment.Left; TPInputTab.Parent = TPPlayerFrame

local TPBtnTab = Instance.new("TextButton")
TPBtnTab.Size = UDim2.new(0, 60, 0, 30); TPBtnTab.Position = UDim2.new(1, -70, 0, 7)
TPBtnTab.BackgroundColor3 = C.primary; TPBtnTab.Text = "IR"; TPBtnTab.TextColor3 = C.text
TPBtnTab.TextSize = 12; TPBtnTab.Font = Enum.Font.GothamBold; TPBtnTab.Parent = TPPlayerFrame
Instance.new("UICorner", TPBtnTab).CornerRadius = UDim.new(0, 15)

TPBtnTab.MouseButton1Click:Connect(function()
    local name = TPInputTab.Text:lower(); local found = false
    if name ~= "" then
        for _, p in pairs(Players:GetPlayers()) do
            if p.Name:lower():sub(1, #name) == name and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                    LP.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame
                    Notify("Teleportado para " .. p.Name); found = true
                end
                break
            end
        end
        if not found then Notify("Player não encontrado!") end
    end
end)

-- ==================== ABA 4: VISUAIS & FUN ====================
local RgTog, RgKnob, RgBtn = createToggle("Rainbow Char (RGB)", tabContainers[4], 0)
local TrTog, TrKnob, TrBtn = createToggle("Rastro Colorido (Trail)", tabContainers[4], 48)
local DiTog, DiKnob, DiBtn = createToggle("Disco Sky", tabContainers[4], 96)
local SpnTog, SpnKnob, SpnBtn = createToggle("Spinbot", tabContainers[4], 144)
local MgTog, MgKnob, MgBtn = createToggle("Moon Gravity", tabContainers[4], 192)

local hue = 0
table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.Rainbow and LP.Character then
        hue = hue + 0.005; if hue >= 1 then hue = 0 end
        local color = Color3.fromHSV(hue, 1, 1)
        for _, v in pairs(LP.Character:GetDescendants()) do
            if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then v.Color = color end
        end
    end
end))
RgBtn.MouseButton1Click:Connect(function() NERO.Rainbow = not NERO.Rainbow; updateToggle(RgTog, RgKnob, NERO.Rainbow) end)

local currentTrail, att0, att1
TrBtn.MouseButton1Click:Connect(function()
    NERO.Trail = not NERO.Trail; updateToggle(TrTog, TrKnob, NERO.Trail)
    if NERO.Trail and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        att0 = Instance.new("Attachment", hrp); att0.Position = Vector3.new(0, 1, 0)
        att1 = Instance.new("Attachment", hrp); att1.Position = Vector3.new(0, -1.2, 0)
        currentTrail = Instance.new("Trail", hrp); currentTrail.Attachment0 = att0; currentTrail.Attachment1 = att1
        currentTrail.Lifetime = 0.8; currentTrail.LightEmission = 0.5
        currentTrail.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 0)), ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 255))})
    else
        if currentTrail then currentTrail:Destroy() end
        if att0 then att0:Destroy() end
        if att1 then att1:Destroy() end
    end
end)

local discoHue = 0; local defaultAmbient = Lighting.Ambient
table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.DiscoSky then
        discoHue = discoHue + 0.002; if discoHue > 1 then discoHue = 0 end
        Lighting.Ambient = Color3.fromHSV(discoHue, 1, 1)
    else Lighting.Ambient = defaultAmbient end
end))
DiBtn.MouseButton1Click:Connect(function() NERO.DiscoSky = not NERO.DiscoSky; updateToggle(DiTog, DiKnob, NERO.DiscoSky) end)

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if NERO.Spinbot and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(25), 0)
    end
end))
SpnBtn.MouseButton1Click:Connect(function() NERO.Spinbot = not NERO.Spinbot; updateToggle(SpnTog, SpnKnob, NERO.Spinbot) end)

local defaultGravity = workspace.Gravity
MgBtn.MouseButton1Click:Connect(function() 
    NERO.MoonGravity = not NERO.MoonGravity; updateToggle(MgTog, MgKnob, NERO.MoonGravity)
    if NERO.MoonGravity then workspace.Gravity = 45 else workspace.Gravity = defaultGravity end
end)

-- ==================== ABA 5: CRÉDITOS ====================
local CTBackground = Instance.new("Frame")
CTBackground.Size = UDim2.new(1, 0, 0, 160); CTBackground.Position = UDim2.new(0, 0, 0, 20)
CTBackground.BackgroundColor3 = Color3.fromRGB(20, 22, 25); CTBackground.Parent = tabContainers[5]
Instance.new("UICorner", CTBackground).CornerRadius = UDim.new(0, 20)
local ctStroke = Instance.new("UIStroke", CTBackground); ctStroke.Color = C.primary; ctStroke.Thickness = 2

local CTTitle = Instance.new("TextLabel")
CTTitle.Size = UDim2.new(1, 0, 0, 40); CTTitle.Position = UDim2.new(0, 0, 0, 10)
CTTitle.BackgroundTransparency = 1; CTTitle.Text = "👑 NERO FE v20.0 👑"
CTTitle.TextColor3 = C.premium; CTTitle.Font = Enum.Font.GothamBold; CTTitle.TextSize = 22; CTTitle.Parent = CTBackground

local CTSub = Instance.new("TextLabel")
CTSub.Size = UDim2.new(1, 0, 0, 30); CTSub.Position = UDim2.new(0, 0, 0, 50)
CTSub.BackgroundTransparency = 1; CTSub.Text = "OMNI PREMIUM EDITION"
CTSub.TextColor3 = Color3.fromRGB(200, 200, 200); CTSub.Font = Enum.Font.GothamMedium; CTSub.TextSize = 12; CTSub.Parent = CTBackground

local CTAuthor = Instance.new("TextLabel")
CTAuthor.Size = UDim2.new(1, 0, 0, 40); CTAuthor.Position = UDim2.new(0, 0, 0, 95)
CTAuthor.BackgroundTransparency = 1; CTAuthor.Text = "Criado e Idealizado por:\nDark & DemonFrota"
CTAuthor.TextColor3 = C.primary; CTAuthor.Font = Enum.Font.GothamBold; CTAuthor.TextSize = 16; CTAuthor.Parent = CTBackground

-- ==================== ABA 6: CONFIG ====================
local function createSlider(name, y, min, max, val, set)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 44); frame.Position = UDim2.new(0, 0, 0, y)
    frame.BackgroundColor3 = C.surface; frame.Parent = tabContainers[6]
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 20)
    local s = Instance.new("UIStroke", frame); s.Color = C.primary; s.Transparency = 0.7

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 150, 1, 0); label.Position = UDim2.new(0, 15, 0, 0)
    label.BackgroundTransparency = 1; label.Text = name .. ": " .. val
    label.TextColor3 = C.text; label.TextSize = 12; label.Font = Enum.Font.GothamMedium
    label.TextXAlignment = Enum.TextXAlignment.Left; label.Parent = frame

    local input = Instance.new("TextBox")
    input.Size = UDim2.new(0, 60, 0, 26); input.Position = UDim2.new(1, -75, 0, 9)
    input.BackgroundColor3 = C.bg; input.TextColor3 = C.primary; input.TextSize = 12
    input.Font = Enum.Font.GothamBold; input.Text = tostring(val); input.Parent = frame
    Instance.new("UICorner", input).CornerRadius = UDim.new(0, 13)
    input.FocusLost:Connect(function()
        local v = tonumber(input.Text)
        if v then v = math.clamp(v, min, max); input.Text = tostring(v); label.Text = name .. ": " .. v; set(v) end
    end)
end
createSlider("FOV Area", 0, 30, 500, NERO.FOV, function(v) NERO.FOV = v end)
createSlider("Velocidade", 48, 16, 500, NERO.SpeedVal, function(v) NERO.SpeedVal = v end)

createTabButton("⚠️ Fechar e Limpar Script", tabContainers[6], 96, function()
    for _, conn in pairs(NERO.Connections) do pcall(function() conn:Disconnect() end) end
    if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.WalkSpeed = 16 end
    workspace.Gravity = defaultGravity; Lighting.Ambient = defaultAmbient; Gui:Destroy()
    Notify("Script Finalizado com Sucesso.")
end)

-- ==================== ABA 7: 🌌 OMNI ====================
local OmniHeader = Instance.new("Frame")
OmniHeader.Size = UDim2.new(1, 0, 0, 40); OmniHeader.Position = UDim2.new(0, 0, 0, 0)
OmniHeader.BackgroundColor3 = Color3.fromRGB(20, 20, 25); OmniHeader.Parent = tabContainers[7]
Instance.new("UICorner", OmniHeader).CornerRadius = UDim.new(0, 12)
local OmniHeaderStroke = Instance.new("UIStroke", OmniHeader); OmniHeaderStroke.Color = C.premium; OmniHeaderStroke.Thickness = 1

local OmniHeaderText = Instance.new("TextLabel")
OmniHeaderText.Size = UDim2.new(1, 0, 1, 0); OmniHeaderText.BackgroundTransparency = 1
OmniHeaderText.Text = "⚡ ADAPTADOR ACTIVATED: " .. GameName:upper()
OmniHeaderText.TextColor3 = C.premium; OmniHeaderText.Font = Enum.Font.GothamBold; OmniHeaderText.TextSize = 10; OmniHeaderText.Parent = OmniHeader

local Q_Tog, Q_Knob, Q_Btn = createPremiumToggle("Nexus Quantum Trigger", "Auto-interagir instantâneo com tudo em volta (Server-side)", tabContainers[7], 50)
local A_Tog, A_Knob, A_Btn = createPremiumToggle("Aether Vacuum Magnet", "Coleta drops, baús e itens soltos fisicamente no mapa", tabContainers[7], 106)
local C_Tog, C_Knob, C_Btn = createPremiumToggle("Chronos Click Overdrive", "Clika automaticamente em botões físicos (ClickDetectors)", tabContainers[7], 162)
local M_Tog, M_Knob, M_Btn = createPremiumToggle("Matrix Desync Shifter", "Modifica física e pacotes de rede para esquivar de ataques", tabContainers[7], 218)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.QuantumTrigger then
        for _, prompt in pairs(workspace:GetDescendants()) do
            if prompt:IsA("ProximityPrompt") then
                pcall(function()
                    if LP:DistanceFromCharacter(prompt.Parent:GetPivot().Position) <= (prompt.MaxActivationDistance + 10) then fireproximityprompt(prompt) end
                end)
            end
        end
    end
end))
Q_Btn.MouseButton1Click:Connect(function() NERO.QuantumTrigger = not NERO.QuantumTrigger; updateToggle(Q_Tog, Q_Knob, NERO.QuantumTrigger, true); if NERO.QuantumTrigger then PremiumNotify("Quantum Trigger", "Varredura ativada!") end end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.AetherMagnet and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        for _, part in pairs(workspace:GetDescendants()) do
            if part:IsA("TouchTransmitter") and part.Parent and part.Parent:IsA("BasePart") then
                pcall(function()
                    if LP:DistanceFromCharacter(part.Parent.Position) < 120 then firetouchinterest(hrp, part.Parent, 0); task.wait(0.01); firetouchinterest(hrp, part.Parent, 1) end
                end)
            end
        end
    end
end))
A_Btn.MouseButton1Click:Connect(function() NERO.AetherMagnet = not NERO.AetherMagnet; updateToggle(A_Tog, A_Knob, NERO.AetherMagnet, true); if NERO.AetherMagnet then PremiumNotify("Vacuum Magnet", "Coletor sincronizado.") end end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.ChronosOverdrive then
        for _, clicker in pairs(workspace:GetDescendants()) do
            if clicker:IsA("ClickDetector") then
                pcall(function() if LP:DistanceFromCharacter(clicker.Parent:GetPivot().Position) <= clicker.MaxActivationDistance then fireclickdetector(clicker) end end)
            end
        end
    end
end))
C_Btn.MouseButton1Click:Connect(function() NERO.ChronosOverdrive = not NERO.ChronosOverdrive; updateToggle(C_Tog, C_Knob, NERO.ChronosOverdrive, true); if NERO.ChronosOverdrive then PremiumNotify("Click Overdrive", "Engatado.") end end)

local desyncFlip = false
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.MatrixDesync and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        desyncFlip = not desyncFlip
        pcall(function()
            local hrp = LP.Character.HumanoidRootPart
            if desyncFlip then hrp.Velocity = Vector3.new(0, -999, 0) else hrp.Velocity = Vector3.new(0, 999, 0) end
        end)
    end
end))
M_Btn.MouseButton1Click:Connect(function() NERO.MatrixDesync = not NERO.MatrixDesync; updateToggle(M_Tog, M_Knob, NERO.MatrixDesync, true); if NERO.MatrixDesync then PremiumNotify("Desync Matrix", "Hitbox dessincronizada.") end end)

local SpecName1, SpecDesc1, SpecName2, SpecDesc2 = "Horizon Velocity Glide", "Deslizar pelas paredes sem fricção", "Quantum Core Scanner", "Inspeciona canais ocultos"
if GameName == "Blox Fruits" then
    SpecName1 = "Aura Chest Vortex"; SpecDesc1 = "Encontra o baú mais próximo e teleporta com segurança"
    SpecName2 = "Combat Instinct Predictor"; SpecDesc2 = "Desvia de skills detectando frames de ataque"
elseif GameName == "Brookhaven RP" then
    SpecName1 = "Identity Matrix Erase"; SpecDesc1 = "Remove dados do avatar do servidor para bugar"
    SpecName2 = "Vortex Estate Hijacker"; SpecDesc2 = "Desativa sistemas de segurança de propriedades"
elseif GameName == "Blade Ball" then
    SpecName1 = "Kinetic Parry Deflector"; SpecDesc1 = "Sincronizador quântico preventivo para bola"
    SpecName2 = "Temporal Phase Shift"; SpecDesc2 = "Modifica o ping aparente para estender o parry"
elseif GameName == "BedWars" then
    SpecName1 = "Vortex Shop Bypass"; SpecDesc1 = "Abre a loja de qualquer lugar do mapa remotamente"
    SpecName2 = "Aura Bed Annihilator"; SpecDesc2 = "Quebra camas invisivelmente através de paredes"
end

local S1_Tog, S1_Knob, S1_Btn = createPremiumToggle(SpecName1, SpecDesc1, tabContainers[7], 274)
local S2_Tog, S2_Knob, S2_Btn = createPremiumToggle(SpecName2, SpecDesc2, tabContainers[7], 330)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.GameSpecificMod1 then
        if GameName == "Blox Fruits" then
            pcall(function() for _, v in pairs(workspace:GetChildren()) do if v.Name:find("Chest") and v:IsA("BasePart") then LP.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0, 2, 0); break end end end)
        elseif GameName == "Brookhaven RP" then
            pcall(function() if LP.Character then for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("StringValue") or v:IsA("ObjectValue") then v:Destroy() end end end end)
        else
            pcall(function() if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.PlatformStand = true; LP.Character.HumanoidRootPart.Velocity = LP.Character.HumanoidRootPart.CFrame.LookVector * 100 end end)
        end
    else
        if GameName ~= "Blox Fruits" and GameName ~= "Brookhaven RP" and LP.Character and LP.Character:FindFirstChild("Humanoid") then pcall(function() LP.Character.Humanoid.PlatformStand = false end) end
    end
end))
S1_Btn.MouseButton1Click:Connect(function() NERO.GameSpecificMod1 = not NERO.GameSpecificMod1; updateToggle(S1_Tog, S1_Knob, NERO.GameSpecificMod1, true); if NERO.GameSpecificMod1 then PremiumNotify(SpecName1, "Ativado!") end end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.GameSpecificMod2 then
        if GameName == "Blox Fruits" then
            pcall(function() if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0.2) end end)
        elseif GameName == "BedWars" then
            pcall(function() for _, v in pairs(workspace:GetDescendants()) do if v.Name:lower():find("bed") and v:IsA("BasePart") then if LP:DistanceFromCharacter(v.Position) < 25 then v:Destroy() end end end end)
        end
    end
end))
S2_Btn.MouseButton1Click:Connect(function() NERO.GameSpecificMod2 = not NERO.GameSpecificMod2; updateToggle(S2_Tog, S2_Knob, NERO.GameSpecificMod2, true); if NERO.GameSpecificMod2 then PremiumNotify(SpecName2, "Carregado.") end end)

-- ==================== ABA 8: 🔥 CAOS ====================
local BhTog, BhKnob, BhBtn = createToggle("Buraco Negro (Itens Soltos)", tabContainers[8], 0)
local MdTog, MdKnob, MdBtn = createToggle("Toque de Midas (Tudo Ouro)", tabContainers[8], 48)
local FaTog, FaKnob, FaBtn = createToggle("Fling Spinbot (Ciclone)", tabContainers[8], 96)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Blackhole and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") and not v.Anchored and not v:IsDescendantOf(LP.Character) then
                if not Players:GetPlayerFromCharacter(v.Parent) then
                    pcall(function() v.CFrame = LP.Character.HumanoidRootPart.CFrame end)
                end
            end
        end
    end
end))
BhBtn.MouseButton1Click:Connect(function() NERO.Blackhole = not NERO.Blackhole; updateToggle(BhTog, BhKnob, NERO.Blackhole) end)

local midasConn
MdBtn.MouseButton1Click:Connect(function()
    NERO.Midas = not NERO.Midas
    updateToggle(MdTog, MdKnob, NERO.Midas)
    if NERO.Midas and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        midasConn = LP.Character.HumanoidRootPart.Touched:Connect(function(hit)
            if not hit:IsDescendantOf(LP.Character) and hit:IsA("BasePart") then
                hit.Material = Enum.Material.Neon; hit.Color = Color3.fromRGB(255, 215, 0)
            end
        end)
    else
        if midasConn then midasConn:Disconnect() end
    end
end)

local auraBody
FaBtn.MouseButton1Click:Connect(function()
    NERO.FlingAura = not NERO.FlingAura
    updateToggle(FaTog, FaKnob, NERO.FlingAura)
    if NERO.FlingAura and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        auraBody = Instance.new("BodyAngularVelocity")
        auraBody.MaxTorque = Vector3.new(0, math.huge, 0)
        auraBody.AngularVelocity = Vector3.new(0, 15000, 0)
        auraBody.Parent = LP.Character.HumanoidRootPart
    else
        if auraBody then auraBody:Destroy() end
    end
end)

-- ==================== ABA 9: 🧠 LÓGICA ====================
local TsTog, TsKnob, TsBtn = createToggle("Za Warudo (Congelar Mapa)", tabContainers[9], 0)
local HxTog, HxKnob, HxBtn = createToggle("Hitbox Expander Global", tabContainers[9], 48)
local UiTog, UiKnob, UiBtn = createToggle("Instinto Superior (Esquiva)", tabContainers[9], 96)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.TimeStop then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Anchored = true
            end
        end
    else
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Anchored = false
            end
        end
    end
end))
TsBtn.MouseButton1Click:Connect(function() NERO.TimeStop = not NERO.TimeStop; updateToggle(TsTog, TsKnob, NERO.TimeStop) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Hitbox then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                p.Character.HumanoidRootPart.Size = Vector3.new(15, 15, 15)
                p.Character.HumanoidRootPart.Transparency = 0.6
                p.Character.HumanoidRootPart.BrickColor = BrickColor.new("Bright blue")
            end
        end
    end
end))
HxBtn.MouseButton1Click:Connect(function() NERO.Hitbox = not NERO.Hitbox; updateToggle(HxTog, HxKnob, NERO.Hitbox) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.UltraInstinct and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local dist = (p.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                if dist > 0 and dist < 8 then
                    LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 15)
                end
            end
        end
    end
end))
UiBtn.MouseButton1Click:Connect(function() NERO.UltraInstinct = not NERO.UltraInstinct; updateToggle(UiTog, UiKnob, NERO.UltraInstinct) end)

-- ==================== ABA 10: 🎭 ILUSÃO ====================
createTabButton("Clonar a si mesmo (Jutsu)", tabContainers[10], 0, function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.Archivable = true
        local clone = LP.Character:Clone()
        clone.Parent = workspace
        clone.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
        clone.Humanoid:MoveTo(clone.HumanoidRootPart.Position + (clone.HumanoidRootPart.CFrame.LookVector * 100))
        game.Debris:AddItem(clone, 6); Notify("Clone criado!")
    end
end)

local FlTog, FlKnob, FlBtn = createToggle("Fake Lag (Bugar Visão)", tabContainers[10], 48)
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.FakeLag and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        LP.Character.HumanoidRootPart.Anchored = true; task.wait(0.2)
        LP.Character.HumanoidRootPart.Anchored = false; task.wait(0.5)
    end
end))
FlBtn.MouseButton1Click:Connect(function() NERO.FakeLag = not NERO.FakeLag; updateToggle(FlTog, FlKnob, NERO.FakeLag) end)

createTabButton("Céu Apocalíptico", tabContainers[10], 96, function()
    Lighting.TimeOfDay = "00:00:00"; Lighting.FogColor = Color3.fromRGB(255, 0, 0)
    Lighting.FogEnd = 200; Lighting.Ambient = Color3.fromRGB(255, 50, 50)
    Notify("Clima alterado localmente!")
end)


-- ==================== ABA 11: 💣 EXTREMO (VISÍVEL PARA TODOS) ====================
local FAllTog, FAllKnob, FAllBtn = createToggle("Fling All (Física Replicada)", tabContainers[11], 0)
local GWTog, GWKnob, GWBtn = createToggle("Glitch Avatar (Convulsão FE)", tabContainers[11], 48)

local flingAllConn
FAllBtn.MouseButton1Click:Connect(function()
    NERO.FlingAll = not NERO.FlingAll
    updateToggle(FAllTog, FAllKnob, NERO.FlingAll)
    if NERO.FlingAll then
        local bav = Instance.new("BodyAngularVelocity")
        bav.Name = "NERO_Fling_Bav"
        bav.AngularVelocity = Vector3.new(0, 99999, 0)
        bav.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then bav.Parent = LP.Character.HumanoidRootPart end
        
        flingAllConn = RS.Heartbeat:Connect(function()
            if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = LP.Character.HumanoidRootPart
                for _, p in pairs(Players:GetPlayers()) do
                    if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                        hrp.CFrame = p.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0.5)
                        task.wait(0.05)
                    end
                end
            end
        end)
    else
        if flingAllConn then flingAllConn:Disconnect() end
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local b = LP.Character.HumanoidRootPart:FindFirstChild("NERO_Fling_Bav")
            if b then b:Destroy() end
        end
    end
end)

local glitchConn
GWBtn.MouseButton1Click:Connect(function()
    NERO.GlitchWalk = not NERO.GlitchWalk
    updateToggle(GWTog, GWKnob, NERO.GlitchWalk)
    if NERO.GlitchWalk then
        glitchConn = RS.Heartbeat:Connect(function()
            if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") and LP.Character:FindFirstChild("Humanoid") then
                local hrp = LP.Character.HumanoidRootPart
                LP.Character.Humanoid.PlatformStand = not LP.Character.Humanoid.PlatformStand
                hrp.Velocity = Vector3.new(math.random(-50,50), math.random(0,50), math.random(-50,50))
                hrp.CFrame = hrp.CFrame * CFrame.Angles(math.random(-2,2), math.random(-2,2), math.random(-2,2))
            end
        end)
    else
        if glitchConn then glitchConn:Disconnect() end
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.PlatformStand = false end
    end
end)


-- ==================== ABA 12: 🗣️ SOCIAL (INTERAÇÃO DE SERVIDOR) ====================
local CSInput = Instance.new("TextBox")
CSInput.Size = UDim2.new(1, 0, 0, 44); CSInput.Position = UDim2.new(0, 0, 0, 0)
CSInput.BackgroundColor3 = C.surface; CSInput.TextColor3 = C.text
CSInput.TextSize = 12; CSInput.Font = Enum.Font.Gotham; CSInput.PlaceholderText = "Texto para Spammar..."
CSInput.Text = "Nero FE Dominando este servidor!"; CSInput.Parent = tabContainers[12]
Instance.new("UICorner", CSInput).CornerRadius = UDim.new(0, 20)
local csStroke = Instance.new("UIStroke", CSInput); csStroke.Color = C.primary; csStroke.Transparency = 0.7

local CSTog, CSKnob, CSBtn = createToggle("Ativar Chat Spammer", tabContainers[12], 48)

local spamConn
CSBtn.MouseButton1Click:Connect(function()
    NERO.ChatSpam = not NERO.ChatSpam
    updateToggle(CSTog, CSKnob, NERO.ChatSpam)
    if NERO.ChatSpam then
        spamConn = task.spawn(function()
            while NERO.ChatSpam do
                pcall(function()
                    local msg = CSInput.Text
                    if game:GetService("TextChatService").ChatVersion == Enum.ChatVersion.TextChatService then
                        game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync(msg)
                    else
                        game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(msg, "All")
                    end
                end)
                task.wait(1.5)
            end
        end)
    end
end)

createTabButton("Forçar Emote Dança (/e dance)", tabContainers[12], 96, function()
    pcall(function()
        if game:GetService("TextChatService").ChatVersion == Enum.ChatVersion.TextChatService then
            game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync("/e dance3")
        else
            game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/e dance3", "All")
        end
    end)
    Notify("Dança enviada ao chat global!")
end)


-- ==================== LÓGICAS NATIVAS ORIGINAIS (INTACTAS) ====================
local function applyHighlight(player)
    if player == LP then return end
    local function setup(char)
        if not NERO.ESP then return end
        local hl = char:FindFirstChild("NERO_ESP") or Instance.new("Highlight")
        hl.Name = "NERO_ESP"; hl.FillColor = C.primary; hl.FillTransparency = 0.4
        hl.OutlineColor = Color3.fromRGB(255, 255, 255); hl.OutlineTransparency = 0.1
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop; hl.Adornee = char; hl.Parent = char
    end
    if player.Character then setup(player.Character) end
    table.insert(NERO.Connections, player.CharacterAdded:Connect(setup))
end

EBtn.MouseButton1Click:Connect(function()
    NERO.ESP = not NERO.ESP; updateToggle(ETog, EKnob, NERO.ESP)
    for _, p in pairs(Players:GetPlayers()) do
        if NERO.ESP then applyHighlight(p) else
            if p.Character and p.Character:FindFirstChild("NERO_ESP") then p.Character.NERO_ESP:Destroy() end
        end
    end
end)
table.insert(NERO.Connections, Players.PlayerAdded:Connect(applyHighlight))

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if not NERO.Aimbot then return end
    local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    local closestTarget = nil; local closestDist = NERO.FOV

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LP and p.Character and p.Character:FindFirstChild("Head") and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
            local sp, onScreen = Camera:WorldToScreenPoint(p.Character.Head.Position)
            if onScreen then
                local dist = (Vector2.new(sp.X, sp.Y) - center).Magnitude
                if dist < closestDist then closestDist = dist; closestTarget = p.Character.Head end
            end
        end
    end
    if closestTarget then Camera.CFrame = CFrame.new(Camera.CFrame.Position, closestTarget.Position) end
end))
ABtn.MouseButton1Click:Connect(function() NERO.Aimbot = not NERO.Aimbot; updateToggle(ATog, AKnob, NERO.Aimbot) end)

table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if NERO.Imortal and LP.Character then
        local hum = LP.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            if hum.Health < hum.MaxHealth then hum.Health = hum.MaxHealth end
            local state = hum:GetState()
            if state == Enum.HumanoidStateType.Dead or state == Enum.HumanoidStateType.FallingDown then hum:ChangeState(Enum.HumanoidStateType.GettingUp) end
        end
    end
end))
ImBtn.MouseButton1Click:Connect(function() NERO.Imortal = not NERO.Imortal; updateToggle(ImTog, ImKnob, NERO.Imortal) end)

table.insert(NERO.Connections, RS.Stepped:Connect(function()
    if not NERO.Noclip or not LP.Character then return end
    for _, part in pairs(LP.Character:GetDescendants()) do if part:IsA("BasePart") and part.CanCollide then part.CanCollide = false end end
end))
NBtn.MouseButton1Click:Connect(function() NERO.Noclip = not NERO.Noclip; updateToggle(NTog, NKnob, NERO.Noclip) end)

table.insert(NERO.Connections, RS.RenderStepped:Connect(function()
    if LP.Character and LP.Character:FindFirstChild("Humanoid") and NERO.Speed then LP.Character.Humanoid.WalkSpeed = NERO.SpeedVal end
end))
SBtn.MouseButton1Click:Connect(function() 
    NERO.Speed = not NERO.Speed; updateToggle(SpTog, SpKnob, NERO.Speed)
    if not NERO.Speed and LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid.WalkSpeed = 16 end
end)

table.insert(NERO.Connections, UIS.JumpRequest:Connect(function()
    if NERO.Jump and LP.Character and LP.Character:FindFirstChild("Humanoid") then LP.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
end))
JpBtn.MouseButton1Click:Connect(function() NERO.Jump = not NERO.Jump; updateToggle(JpTog, JpKnob, NERO.Jump) end)

local invisibilityLoaded = false; local invisibilityConnection
InvBtn.MouseButton1Click:Connect(function()
    NERO.Invisible = not NERO.Invisible; updateToggle(InvTog, InvKnob, NERO.Invisible)
    if NERO.Invisible then
        if not invisibilityLoaded then
            invisibilityLoaded = true; invisibilityConnection = loadstring(game:HttpGet('https://pastebin.com/raw/3Rnd9rHf'))(); Notify("Sistema de Invisibilidade Ativado!")
        end
    else
        if invisibilityLoaded then
            invisibilityLoaded = false
            if invisibilityConnection then
                pcall(function()
                    if LP.Character then
                        for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") or v:IsA("Decal") then if v.Name ~= "HumanoidRootPart" then v.Transparency = 0 end end end
                    end
                    for _, v in pairs(LP.Character:GetDescendants()) do if v:IsA("Highlight") and v.Name == "InvisibilityHighlight" then v:Destroy() end end
                end)
                invisibilityConnection = nil
            end
            Notify("Sistema de Invisibilidade Desativado!")
        end
    end
end)

local angle = 0
table.insert(NERO.Connections, RS.Heartbeat:Connect(function()
    if (NERO.Backpack or NERO.Beijo or NERO.Tornado) then
        local target = findTarget()
        if target and target:FindFirstChild("Head") and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LP.Character.HumanoidRootPart
            if NERO.Backpack then hrp.CFrame = target.Head.CFrame * CFrame.new(0, 3, 0)
            elseif NERO.Beijo then hrp.CFrame = target.Head.CFrame * CFrame.new(0, 0, -1.5)
            elseif NERO.Tornado then angle = angle + 10; hrp.CFrame = target.Head.CFrame * CFrame.Angles(0, math.rad(angle), 0) * CFrame.new(0, 0, -4) end
            hrp.Velocity = Vector3.new(0,0,0)
        end
    end
end))

BpBtn.MouseButton1Click:Connect(function() NERO.Backpack = not NERO.Backpack; updateToggle(BpTog, BpKnob, NERO.Backpack) end)
BjBtn.MouseButton1Click:Connect(function() NERO.Beijo = not NERO.Beijo; updateToggle(BjTog, BjKnob, NERO.Beijo) end)
ToBtn.MouseButton1Click:Connect(function() NERO.Tornado = not NERO.Tornado; updateToggle(ToTog, ToKnob, NERO.Tornado) end)

LauBtn.MouseButton1Click:Connect(function() 
    NERO.Launch = not NERO.Launch; updateToggle(LauTog, LauKnob, NERO.Launch) 
    if NERO.Launch then Notify("Fling ativado! Chegue perto e aperte 'E' no teclado.") end
end)

table.insert(NERO.Connections, UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if NERO.Launch and input.KeyCode == Enum.KeyCode.E and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        local spin = Instance.new("BodyAngularVelocity")
        spin.Name = "NERO_Spin"; spin.Parent = hrp
        spin.MaxTorque = Vector3.new(math.huge, math.huge, math.huge); spin.AngularVelocity = Vector3.new(0, 10000, 0)
        task.wait(0.5)
        if spin then spin:Destroy() end
    end
end))

FBtn.MouseButton1Click:Connect(function() 
    NERO.Fly = not NERO.Fly; updateToggle(FTog, FKnob, NERO.Fly)
    if NERO.Fly then task.spawn(function() pcall(loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))) end) end 
end)

local NFContainer = Instance.new("Frame")
NFContainer.Size = UDim2.new(0, 50, 0, 50); NFContainer.Position = UDim2.new(0, 20, 0, 20)
NFContainer.BackgroundColor3 = Color3.fromRGB(15, 15, 15); NFContainer.Visible = false; NFContainer.Parent = Gui
Instance.new("UICorner", NFContainer).CornerRadius = UDim.new(0, 25)

local NFStroke = Instance.new("UIStroke", NFContainer)
NFStroke.Color = C.primary; NFStroke.Thickness = 2
TS:Create(NFStroke, TweenInfoPulse, {Transparency = 0.8}):Play()

local NFBtn = Instance.new("TextButton")
NFBtn.Size = UDim2.new(1,0,1,0); NFBtn.BackgroundTransparency = 1; NFBtn.Text = "N"
NFBtn.TextColor3 = C.primary; NFBtn.TextSize = 20; NFBtn.Font = Enum.Font.GothamBold; NFBtn.Parent = NFContainer

CloseBtn.MouseButton1Click:Connect(function() Main.Visible = false; NFContainer.Visible = true end)
NFBtn.MouseButton1Click:Connect(function() Main.Visible = true; NFContainer.Visible = false end)

local function makeSmoothDraggable(obj)
    local dragging, dragInput, dragStart, startPos
    obj.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging, dragStart, startPos = true, input.Position, obj.Position
            input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    obj.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
    end)
    table.insert(NERO.Connections, UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            TS:Create(obj, TweenInfo.new(0.12, Enum.EasingStyle.OutQuad), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
        end
    end))
end

makeSmoothDraggable(Main)
makeSmoothDraggable(NFContainer)

-- MÓDULO BOTÃO VIRTUAL MOBILE (E) INTOCADO
local ScreenGui = targetParent:FindFirstChild("NERO_HTML")
if ScreenGui then
    local EContainer = Instance.new("Frame")
    EContainer.Name = "NERO_E_Button"
    EContainer.Size = UDim2.new(0, 50, 0, 50)
    EContainer.Position = UDim2.new(0.85, 0, 0.5, 0) 
    EContainer.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    EContainer.Visible = false
    EContainer.Active = true
    EContainer.Parent = ScreenGui
    Instance.new("UICorner", EContainer).CornerRadius = UDim.new(0, 25)

    local EStroke = Instance.new("UIStroke", EContainer)
    EStroke.Color = Color3.fromRGB(255, 126, 95)
    EStroke.Thickness = 2
    TS:Create(EStroke, TweenInfoPulse, {Transparency = 0.8}):Play()

    local EBtn = Instance.new("TextButton")
    EBtn.Size = UDim2.new(1, 0, 1, 0)
    EBtn.BackgroundTransparency = 1
    EBtn.Text = "E"
    EBtn.TextColor3 = Color3.fromRGB(255, 126, 95)
    EBtn.TextSize = 20
    EBtn.Font = Enum.Font.GothamBold
    EBtn.Parent = EContainer

    task.spawn(function()
        while task.wait(0.2) do
            if NERO ~= nil and NERO.Launch ~= nil then
                EContainer.Visible = NERO.Launch
            end
        end
    end)

    EBtn.MouseButton1Click:Connect(function()
        if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LP.Character.HumanoidRootPart
            local oldSpin = hrp:FindFirstChild("NERO_Spin")
            if oldSpin then oldSpin:Destroy() end

            local spin = Instance.new("BodyAngularVelocity")
            spin.Name = "NERO_Spin"
            spin.Parent = hrp
            spin.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
            spin.AngularVelocity = Vector3.new(0, 10000, 0)
            task.wait(0.5)
            if spin then spin:Destroy() end
        end
    end)

    local dragging = false
    local dragInput, dragStart, startPos
    EBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = EContainer.Position
            input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
        end
    end)
    EBtn.InputChanged:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end end)
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            TS:Create(EContainer, TweenInfo.new(0.1, Enum.EasingStyle.OutQuad), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
        end
    end)
end

print("========================================")
print(" 👑 [NERO OMNI PREMIUM v20.0] LOADED! 👑 ")
print("      Ambiente Ativo: " .. GameName)
print("========================================")

local sound = Instance.new("Sound", game.Workspace)
sound.SoundId = "rbxassetid://6042053626"; sound.Volume = 1; sound:Play(); game.Debris:AddItem(sound, 3)

task.spawn(function()
    task.wait(0.5)
    Notify("🔥 NERO FE v20.0 Carregado 🔥")
    DispararConfetes(Gui)
    task.wait(1.5)
    PremiumNotify("OMNI ENGINE ACTIVATED", "Módulo Premium adaptado para: " .. GameName)
end)
 -- ==========================================================
-- 🚀 MÓDULO SECRETO NERO: OMNIVERSAL GLITCH (RTX UI - CORRIGIDA) 🚀
-- ==========================================================

task.spawn(function()
    -- 1. Encontrar o botão de Créditos no seu script
    local creditosBtn = nil
    for _, btn in ipairs(tabs) do
        if btn.Text == "CRÉDITOS" then
            creditosBtn = btn
            break
        end
    end

    if not creditosBtn then return end

    -- 2. Lógica de 3 Cliques Rápidos
    local clickCount = 0
    local lastClickTime = 0

    creditosBtn.MouseButton1Click:Connect(function()
        local now = tick()
        if now - lastClickTime < 0.4 then
            clickCount = clickCount + 1
        else
            clickCount = 1
        end
        lastClickTime = now

        if clickCount == 3 then
            clickCount = 0
            AtivarInterfaceSecreta()
        end
    end)

    -- 3. Função Principal da UI RTX (3% Menor para Celular)
    function AtivarInterfaceSecreta()
        -- Esconde a UI principal
        if Main then Main.Visible = false end

        local SecretGui = Instance.new("ScreenGui")
        SecretGui.Name = "NERO_SECRET_RTX"
        SecretGui.ResetOnSpawn = false
        SecretGui.Parent = targetParent
        SecretGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

        -- ==================== TELA DE ERRO GLITCH ====================
        local ErrorFrame = Instance.new("Frame")
        ErrorFrame.Size = UDim2.new(1, 0, 1, 0)
        ErrorFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        ErrorFrame.Parent = SecretGui

        local ErrorText = Instance.new("TextLabel")
        ErrorText.Size = UDim2.new(1, 0, 1, 0)
        ErrorText.BackgroundTransparency = 1
        ErrorText.Text = "ERRO ERRO ERRO ERRO ERRO ERRO ERRO ERRO ERRO ERRO ERRO ERRO"
        ErrorText.TextColor3 = Color3.fromRGB(255, 0, 0)
        ErrorText.Font = Enum.Font.GothamBlack
        ErrorText.TextSize = 50
        ErrorText.TextWrapped = true
        ErrorText.Parent = ErrorFrame

        -- Som de Glitch Assustador
        local glitchSound = Instance.new("Sound", workspace)
        glitchSound.SoundId = "rbxassetid://835235282"
        glitchSound.Volume = 2
        glitchSound:Play()

        -- Animação de Glitch Brutal
        for i = 1, 25 do
            ErrorText.Position = UDim2.new(0, math.random(-20, 20), 0, math.random(-20, 20))
            ErrorText.TextColor3 = (i % 2 == 0) and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 255, 255)
            ErrorText.TextSize = math.random(40, 70)
            task.wait(0.05)
        end
        ErrorText.Position = UDim2.new(0,0,0,0)
        ErrorText.TextColor3 = Color3.fromRGB(255, 0, 0)
        task.wait(1)

        -- ==================== TRANSIÇÃO PARA A UI RTX ====================
        TS:Create(ErrorFrame, TweenInfo.new(1, Enum.EasingStyle.Exponential, Enum.EasingDirection.Out), {BackgroundTransparency = 1}):Play()
        TS:Create(ErrorText, TweenInfo.new(1), {TextTransparency = 1}):Play()
        task.wait(1)
        ErrorFrame:Destroy()

        -- [AJUSTADO]: Janela recalculada para 680x480 (Perfeita para cliques nas bordas)
        local RTXMain = Instance.new("Frame")
        RTXMain.Size = UDim2.new(0, 680, 0, 480)
        RTXMain.Position = UDim2.new(0.5, -340, 0.5, -240)
        RTXMain.BackgroundColor3 = Color3.fromRGB(8, 8, 12)
        RTXMain.BorderSizePixel = 0
        RTXMain.ClipsDescendants = true
        RTXMain.Parent = SecretGui
        Instance.new("UICorner", RTXMain).CornerRadius = UDim.new(0, 15)

        -- Borda Brilhante Neon (Cyberpunk/RTX Style)
        local RTXStroke = Instance.new("UIStroke", RTXMain)
        RTXStroke.Thickness = 3
        RTXStroke.Color = Color3.fromRGB(255, 255, 255)
        local RTXGradient = Instance.new("UIGradient", RTXStroke)
        RTXGradient.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 255)),
            ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 0, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 255, 0))
        })

        -- Animar Gradiente infinitamente
        task.spawn(function()
            local rot = 0
            while task.wait(0.01) do
                rot = rot + 2
                if rot >= 360 then rot = 0 end
                RTXGradient.Rotation = rot
            end
        end)

        -- Botão Fechar (Agora super acessível)
        local SecClose = Instance.new("TextButton")
        SecClose.Size = UDim2.new(0, 35, 0, 35)
        SecClose.Position = UDim2.new(1, -45, 0, 10)
        SecClose.BackgroundTransparency = 1
        SecClose.Text = "X"
        SecClose.TextColor3 = Color3.fromRGB(255, 50, 50)
        SecClose.Font = Enum.Font.GothamBold
        SecClose.TextSize = 22
        SecClose.Parent = RTXMain
        SecClose.MouseButton1Click:Connect(function()
            SecretGui:Destroy()
            if Main then Main.Visible = true end
        end)

        -- Título da Página Secreta
        local RTXTitle = Instance.new("TextLabel")
        RTXTitle.Size = UDim2.new(1, 0, 0, 50)
        RTXTitle.BackgroundTransparency = 1
        RTXTitle.Text = "🔮 OMNIVERSAL SECRET HUB (100 MODOS) 🔮"
        RTXTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
        RTXTitle.Font = Enum.Font.GothamBlack
        RTXTitle.TextSize = 20
        RTXTitle.Parent = RTXMain
        
        local TitleGradient = Instance.new("UIGradient", RTXTitle)
        TitleGradient.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 255))
        })

        -- Container dos 100 Botões
        local Scroll100 = Instance.new("ScrollingFrame")
        Scroll100.Size = UDim2.new(1, -20, 1, -70)
        Scroll100.Position = UDim2.new(0, 10, 0, 60)
        Scroll100.BackgroundTransparency = 1
        Scroll100.ScrollBarThickness = 4
        Scroll100.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 255)
        Scroll100.Parent = RTXMain

        local Grid = Instance.new("UIGridLayout")
        Grid.CellSize = UDim2.new(0, 205, 0, 60)
        Grid.CellPadding = UDim2.new(0, 10, 0, 10)
        Grid.SortOrder = Enum.SortOrder.LayoutOrder
        Grid.Parent = Scroll100

        -- ==================== GERADOR DOS 100 FE CREATIVOS ====================
        local prefixes = {"Aura", "Distorção", "Domínio", "Colapso", "Overdrive", "Vortex", "Glitch", "Matrix", "Explosão", "Campo"}
        local elements = {"Quântico", "Celestial", "Abissal", "Neon", "Radiante", "Caótico", "Zero-G", "Sangrento", "Cibernético", "Espectral"}
        
        local secretModes = {}

        table.insert(secretModes, {Name = "Desintegração de Thanos", Desc = "Transforma seu corpo em partículas que voam lentamente.", 
            Action = function()
                if LP.Character then
                    for _,v in pairs(LP.Character:GetChildren()) do
                        if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
                            v.Anchored = true
                            local pe = Instance.new("ParticleEmitter", v)
                            pe.Texture = "rbxassetid://243660364"
                            pe.Color = ColorSequence.new(Color3.fromRGB(150, 100, 255))
                            pe.Size = NumberSequence.new({NumberSequenceKeypoint.new(0,0.5), NumberSequenceKeypoint.new(1,0)})
                            TS:Create(v, TweenInfo.new(3), {Transparency = 1}):Play()
                        end
                    end
                end
            end})
        table.insert(secretModes, {Name = "Visão RTX", Desc = "Altera os Shaders para gráficos ultrarrealistas (Client)", 
            Action = function()
                local cc = Instance.new("ColorCorrectionEffect", Lighting)
                cc.Contrast = 0.5; cc.Saturation = 1.5; cc.Brightness = 0.1
                local bloom = Instance.new("BloomEffect", Lighting)
                bloom.Intensity = 1.5; bloom.Size = 40
                local sun = Instance.new("SunRaysEffect", Lighting)
                sun.Intensity = 0.3
            end})
        table.insert(secretModes, {Name = "Gravidade Lunar Invertida", Desc = "Faz todos os jogadores soltos flutuarem para cima", 
            Action = function() workspace.Gravity = -50 end})

        for i = 4, 100 do
            local nome = prefixes[math.random(1, #prefixes)] .. " " .. elements[math.random(1, #elements)] .. " v" .. tostring(math.random(10,99))
            table.insert(secretModes, {
                Name = nome,
                Desc = "Modificação física e visual FE de nível " .. tostring(math.random(1, 5)),
                Action = function()
                    local char = LP.Character
                    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
                    local hrp = char.HumanoidRootPart
                    
                    if nome:find("Aura") or nome:find("Campo") then
                        local att = Instance.new("Attachment", hrp)
                        local pe = Instance.new("ParticleEmitter", att)
                        pe.Color = ColorSequence.new(Color3.fromRGB(math.random(0,255), math.random(0,255), math.random(0,255)))
                        pe.LightEmission = 1
                        pe.Rate = 500; pe.Speed = NumberRange.new(5, 10)
                    end
                    if nome:find("Zero-G") or nome:find("Vortex") then
                        local bv = Instance.new("BodyVelocity", hrp)
                        bv.Velocity = Vector3.new(0, math.random(10, 50), 0)
                        bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                        game.Debris:AddItem(bv, 3)
                    end
                    if nome:find("Glitch") or nome:find("Overdrive") then
                        task.spawn(function()
                            for k=1, 20 do
                                hrp.CFrame = hrp.CFrame * CFrame.Angles(math.random(-3,3), math.random(-3,3), math.random(-3,3))
                                task.wait(0.1)
                            end
                        end)
                    end
                    if nome:find("Cibernético") or nome:find("Neon") then
                        for _, v in pairs(char:GetDescendants()) do
                            if v:IsA("BasePart") then
                                v.Material = Enum.Material.ForceField
                                v.Color = Color3.fromRGB(0, 255, 255)
                            end
                        end
                    end
                end
            })
        end

        for i, mod in ipairs(secretModes) do
            local btn = Instance.new("TextButton")
            btn.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
            btn.Text = ""
            btn.Parent = Scroll100
            Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
            local btnStroke = Instance.new("UIStroke", btn)
            btnStroke.Color = Color3.fromRGB(math.random(50,150), math.random(50,255), math.random(150,255))
            btnStroke.Transparency = 0.5

            local bTitle = Instance.new("TextLabel")
            bTitle.Size = UDim2.new(1, -10, 0, 20)
            bTitle.Position = UDim2.new(0, 5, 0, 5)
            bTitle.BackgroundTransparency = 1
            bTitle.Text = "["..i.."] " .. mod.Name
            bTitle.TextColor3 = Color3.fromRGB(255,255,255)
            bTitle.Font = Enum.Font.GothamBold
            bTitle.TextSize = 11
            bTitle.TextXAlignment = Enum.TextXAlignment.Left
            bTitle.Parent = btn

            local bDesc = Instance.new("TextLabel")
            bDesc.Size = UDim2.new(1, -10, 0, 30)
            bDesc.Position = UDim2.new(0, 5, 0, 25)
            bDesc.BackgroundTransparency = 1
            bDesc.Text = mod.Desc
            bDesc.TextColor3 = Color3.fromRGB(150, 150, 150)
            bDesc.Font = Enum.Font.Gotham
            bDesc.TextSize = 9
            bDesc.TextWrapped = true
            bDesc.TextXAlignment = Enum.TextXAlignment.Left
            bDesc.Parent = btn

            btn.MouseButton1Click:Connect(function()
                TS:Create(btnStroke, TweenInfo.new(0.2), {Color = Color3.fromRGB(255, 255, 255), Thickness = 2}):Play()
                task.wait(0.2)
                TS:Create(btnStroke, TweenInfo.new(0.5), {Color = Color3.fromRGB(0, 255, 255), Thickness = 1}):Play()
                
                Notify("Executando: " .. mod.Name)
                pcall(mod.Action)
            end)
        end
        
        Scroll100.CanvasSize = UDim2.new(0, 0, 0, math.ceil(100 / 3) * 70)
        
        local dragging, dragInput, dragStart, startPos
        RTXMain.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging, dragStart, startPos = true, input.Position, RTXMain.Position
                input.Changed:Connect(function() if input.UserInputState == Enum.UserInputState.End then dragging = false end end)
            end
        end)
        RTXMain.InputChanged:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
        end)
        UIS.InputChanged:Connect(function(input)
            if input == dragInput and dragging then
                local delta = input.Position - dragStart
                TS:Create(RTXMain, TweenInfo.new(0.12, Enum.EasingStyle.OutQuad), {Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)}):Play()
            end
        end)

    end
end)
 -- ==========================================================
-- 💡 MÓDULO LANTERNA AMOLED (PULSANTE + CONTROLE DE DENSIDADE) 💡
-- ==========================================================

task.spawn(function()
    -- 1. Interface Principal
    local LightGui = Instance.new("ScreenGui")
    LightGui.Name = "NERO_AMOLED_FLASHLIGHT"
    LightGui.ResetOnSpawn = false
    LightGui.Parent = targetParent
    LightGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    -- Botão Preto Amoled
    local FloatBtn = Instance.new("ImageButton")
    FloatBtn.Size = UDim2.new(0, 50, 0, 50)
    FloatBtn.Position = UDim2.new(0.85, 0, 0.5, 0)
    FloatBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Preto Amoled Puro
    FloatBtn.Image = "" -- Sem fotos bugadas
    FloatBtn.Visible = false
    FloatBtn.Parent = LightGui
    
    local FloatCorner = Instance.new("UICorner", FloatBtn)
    FloatCorner.CornerRadius = UDim.new(1, 0)
    
    -- O ícone original da Lanterna (Emoji 🔆)
    local FlashlightIcon = Instance.new("TextLabel")
    FlashlightIcon.Size = UDim2.new(0.6, 0, 0.6, 0)
    FlashlightIcon.Position = UDim2.new(0.2, 0, 0.2, 0)
    FlashlightIcon.BackgroundTransparency = 1
    FlashlightIcon.Text = "🔆"
    FlashlightIcon.TextScaled = true
    FlashlightIcon.Parent = FloatBtn
    
    -- Arco Pulsante
    local FloatStroke = Instance.new("UIStroke", FloatBtn)
    FloatStroke.Color = Color3.fromRGB(255, 255, 255)
    FloatStroke.Thickness = 2.5
    
    -- Gradiente do Arco (Fogo / Lanterna)
    local PulseGradient = Instance.new("UIGradient", FloatStroke)
    PulseGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 100, 0)),
        ColorSequenceKeypoint.new(0.5, Color3.fromRGB(255, 200, 0)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 50, 0))
    })

    -- Animação do Arco Pulsante
    task.spawn(function()
        local rot = 0
        while task.wait(0.02) do
            rot = (rot + 4) % 360
            PulseGradient.Rotation = rot
            FloatStroke.Transparency = 0.1 + math.abs(math.sin(tick() * 3)) * 0.6
        end
    end)

    -- 2. Mini Painel Bonitinho de Densidade (Slider)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(0, 160, 0, 60)
    SliderFrame.Position = UDim2.new(0.5, -80, 0.4, 0)
    SliderFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    SliderFrame.Visible = false
    SliderFrame.Parent = LightGui
    Instance.new("UICorner", SliderFrame).CornerRadius = UDim.new(0, 12)
    Instance.new("UIStroke", SliderFrame).Color = Color3.fromRGB(255, 150, 0)

    local SliderTitle = Instance.new("TextLabel")
    SliderTitle.Size = UDim2.new(1, 0, 0, 20)
    SliderTitle.Position = UDim2.new(0, 0, 0, 5)
    SliderTitle.BackgroundTransparency = 1
    SliderTitle.Text = "Densidade da Luz"
    SliderTitle.TextColor3 = Color3.fromRGB(255, 200, 0)
    SliderTitle.Font = Enum.Font.GothamBold
    SliderTitle.TextSize = 12
    SliderTitle.Parent = SliderFrame

    local SliderBg = Instance.new("TextButton")
    SliderBg.Size = UDim2.new(0, 130, 0, 10)
    SliderBg.Position = UDim2.new(0.5, -65, 0, 35)
    SliderBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    SliderBg.Text = ""
    SliderBg.Parent = SliderFrame
    Instance.new("UICorner", SliderBg).CornerRadius = UDim.new(1, 0)

    local SliderFill = Instance.new("Frame")
    SliderFill.Size = UDim2.new(0.8, 0, 1, 0)
    SliderFill.BackgroundColor3 = Color3.fromRGB(255, 150, 0)
    SliderFill.Parent = SliderBg
    Instance.new("UICorner", SliderFill).CornerRadius = UDim.new(1, 0)

    local CloseSlider = Instance.new("TextButton")
    CloseSlider.Size = UDim2.new(0, 25, 0, 25)
    CloseSlider.Position = UDim2.new(1, -10, 0, -10)
    CloseSlider.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    CloseSlider.Text = "X"
    CloseSlider.TextColor3 = Color3.fromRGB(255,255,255)
    CloseSlider.Font = Enum.Font.GothamBold
    CloseSlider.Parent = SliderFrame
    Instance.new("UICorner", CloseSlider).CornerRadius = UDim.new(1, 0)
    
    CloseSlider.MouseButton1Click:Connect(function()
        SliderFrame.Visible = false
    end)

    -- 3. Variáveis e Controle do Slider
    local lightEnabled = false
    local spotLight = nil
    local currentDensity = 40

    local draggingSlider = false
    SliderBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingSlider = true
        end
    end)
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            draggingSlider = false
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if draggingSlider and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local relativeX = math.clamp(input.Position.X - SliderBg.AbsolutePosition.X, 0, SliderBg.AbsoluteSize.X)
            local percentage = relativeX / SliderBg.AbsoluteSize.X
            
            SliderFill.Size = UDim2.new(percentage, 0, 1, 0)
            currentDensity = percentage * 60
            
            if spotLight then
                spotLight.Brightness = currentDensity
            end
        end
    end)

    -- 4. Detecção de Clique, Arraste e 3 Segundos Pressionado
    local isHolding = false
    local holdStartTick = 0
    local holdThread = nil
    local draggingBtn = false
    local startPosBtn, dragStartInput

    FloatBtn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isHolding = true
            holdStartTick = tick()
            draggingBtn = false
            startPosBtn = FloatBtn.Position
            dragStartInput = input.Position

            holdThread = task.spawn(function()
                task.wait(3)
                if isHolding and not draggingBtn then
                    SliderFrame.Visible = true
                    Notify("Controle de Densidade Aberto!")
                    local sfx = Instance.new("Sound", workspace)
                    sfx.SoundId = "rbxassetid://2550619586"
                    sfx.Volume = 1
                    sfx:Play(); game.Debris:AddItem(sfx, 2)
                end
            end)
        end
    end)

    FloatBtn.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            if isHolding and (input.Position - dragStartInput).Magnitude > 10 then
                draggingBtn = true
                if holdThread then task.cancel(holdThread) end
            end
        end
    end)

    UIS.InputChanged:Connect(function(input)
        if draggingBtn and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStartInput
            FloatBtn.Position = UDim2.new(startPosBtn.X.Scale, startPosBtn.X.Offset + delta.X, startPosBtn.Y.Scale, startPosBtn.Y.Offset + delta.Y)
        end
    end)

    FloatBtn.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isHolding = false
            if holdThread then task.cancel(holdThread) end
            
            if tick() - holdStartTick < 3 and not draggingBtn then
                local char = LP.Character
                if not char or not char:FindFirstChild("HumanoidRootPart") then return end
                
                lightEnabled = not lightEnabled

                if lightEnabled then
                    FloatBtn.ImageColor3 = Color3.fromRGB(255, 200, 100)
                    
                    if not spotLight then
                        spotLight = Instance.new("SpotLight")
                        spotLight.Name = "NeroUltraLight"
                        spotLight.Brightness = currentDensity
                        spotLight.Range = 120
                        spotLight.Angle = 70
                        spotLight.Color = Color3.fromRGB(255, 255, 255)
                        spotLight.Face = Enum.NormalId.Front
                        spotLight.Shadows = true
                        spotLight.Parent = char.HumanoidRootPart
                    end

                    for _, part in pairs(char:GetChildren()) do
                        if part:IsA("BasePart") then part.Material = Enum.Material.Neon end
                    end
                else
                    FloatBtn.ImageColor3 = Color3.fromRGB(255, 255, 255)
                    
                    if spotLight then
                        spotLight:Destroy()
                        spotLight = nil
                    end
                    for _, part in pairs(char:GetChildren()) do
                        if part:IsA("BasePart") then part.Material = Enum.Material.Plastic end
                    end
                end
            end
        end
    end)

    -- Visibilidade apenas na Aba VISUAIS
    for _, btn in ipairs(tabs) do
        btn.MouseButton1Click:Connect(function()
            if btn.Text == "VISUAIS" then
                FloatBtn.Visible = true
            else
                FloatBtn.Visible = false
                SliderFrame.Visible = false
            end
        end)
    end
    
    LP.CharacterAdded:Connect(function()
        lightEnabled = false
        spotLight = nil
        FloatBtn.ImageColor3 = Color3.fromRGB(255, 255, 255)
    end)
end)
 -- ==================== MÓDULO: SONS E ANIMAÇÕES ONE UI + MÚSICA ====================
local isAnimating = false

-- === MÚSICA DE FUNDO (DOORS) ===
local bgm = Instance.new("Sound")
bgm.SoundId = "rbxassetid://120313972450310" -- O ID do jogo Doors que você pediu
bgm.Looped = true -- Ativa o loop infinito
bgm.Volume = 1.5 -- Volume da música (ajuste se ficar muito alto em relação aos cliques)
bgm.Parent = game:GetService("SoundService")

-- Se o seu menu já começa aberto na tela quando você executa o script, 
-- descomente a linha abaixo tirando os dois traços para a música já iniciar tocando:
-- bgm:Play() 

-- === EFEITOS DE NOTIFICAÇÃO (SAMSUNG STYLE) ===
local function PlayUIEffect(isOpen)
    local sfx = Instance.new("Sound")
    sfx.SoundId = "rbxassetid://6895079853" 
    sfx.Pitch = isOpen and 1.1 or 0.85 
    sfx.Volume = 5 
    sfx.Parent = game:GetService("SoundService")
    sfx:Play()
    game.Debris:AddItem(sfx, 3) 
end

-- Lógica de Fechar
CloseBtn.MouseButton1Click:Connect(function()
    if isAnimating then return end
    isAnimating = true
    
    PlayUIEffect(false) -- Toca o toque de fechar
    bgm:Pause() -- PAUSA A MÚSICA DE FUNDO
    
    local closeTween = TS:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})
    closeTween:Play()
    
    closeTween.Completed:Connect(function()
        Main.Visible = false
        NFContainer.Visible = true
        NFContainer.Size = UDim2.new(0, 0, 0, 0)
        
        TS:Create(NFContainer, TweenInfo.new(0.6, Enum.EasingStyle.Elastic, Enum.EasingDirection.Out), {Size = UDim2.new(0, 50, 0, 50)}):Play()
        isAnimating = false
    end)
end)

-- Lógica de Abrir
NFBtn.MouseButton1Click:Connect(function()
    if isAnimating then return end
    isAnimating = true
    
    PlayUIEffect(true) -- Toca o toque de abrir
    bgm:Resume() -- DESPAUSA E CONTINUA A MÚSICA DE FUNDO DO PONTO QUE PAROU
    
    local hideBtn = TS:Create(NFContainer, TweenInfo.new(0.2, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})
    hideBtn:Play()
    
    hideBtn.Completed:Connect(function()
        NFContainer.Visible = false
        Main.Visible = true
        Main.Size = UDim2.new(0, 0, 0, 0)
        
        local openTween = TS:Create(Main, TweenInfo.new(0.65, Enum.EasingStyle.Elastic, Enum.EasingDirection.Out), {Size = UDim2.new(0, 330, 0, 465)})
        openTween:Play()
        
        openTween.Completed:Connect(function()
            isAnimating = false
        end)
    end)
end)

-- Bônus: Tecla INSERT para abrir/fechar com segurança
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.Insert then
        if Main.Visible then
            for _, conn in pairs(getconnections(CloseBtn.MouseButton1Click)) do conn:Fire() end
        elseif NFContainer.Visible then
            for _, conn in pairs(getconnections(NFBtn.MouseButton1Click)) do conn:Fire() end
        end
    end
end)
 -- ==========================================================
-- 🎵 NERO MUSIC PLAYER v7 – COM BARRA DE PROGRESSO ANIMADA
-- ==========================================================
task.spawn(function()
    local musicList = {
        "94935794334796",
        "106077539420914",
        "95401969908951",
        "78317236576153",
        "76897780069788",
        "1784221294137",
        "110817176848617",
        "139719375902695",
        "136974179670066",
        "139815305627554",
        "140667339171815",
        "140580823167015",
        "140504265985079",
        "139694762285253",
        "140439256760765",
        "138801603792399",
        "135209837340816",
        "72653741821355",
        "111109407506851",
        "103415930328599",
        "1127916915218718842",
        "124170748342120",
        "129176894324463",
        "95697900241820",
        "97192334657561",
        "120569165858495",
        "127096476530496",
        "130104736468172",
        "134178255757517",
        "102827114027930",
        "120477473525627",
        "101098976710405",
        "136716637489655",
        "86517694279595"
    }

    local MusicGui = Instance.new("ScreenGui")
    MusicGui.Name = "NERO_MusicPlayer"
    MusicGui.ResetOnSpawn = false
    MusicGui.Parent = targetParent
    MusicGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    MusicGui.Enabled = false

    -- Player (150x110)
    local Player = Instance.new("Frame")
    Player.Size = UDim2.new(0, 150, 0, 110)
    Player.Position = UDim2.new(0.5, -75, 0.5, -55)
    Player.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Player.BorderSizePixel = 0
    Player.ClipsDescendants = true
    Player.Parent = MusicGui
    local playerCorner = Instance.new("UICorner", Player)
    playerCorner.CornerRadius = UDim.new(0, 18)

    -- Borda laranja (2px)
    local borderStroke = Instance.new("UIStroke", Player)
    borderStroke.Color = Color3.fromRGB(255, 123, 0)
    borderStroke.Thickness = 2
    borderStroke.Transparency = 0

    -- Glow (sombra externa)
    local glow = Instance.new("Frame")
    glow.Size = UDim2.new(1, 0, 1, 0)
    glow.BackgroundTransparency = 1
    glow.Parent = Player
    local glowStroke = Instance.new("UIStroke", glow)
    glowStroke.Color = Color3.fromRGB(255, 120, 0)
    glowStroke.Thickness = 4
    glowStroke.Transparency = 0.7
    glowStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

    -- Inner glow
    local innerGlow = Instance.new("Frame")
    innerGlow.Size = UDim2.new(1, -8, 1, -8)
    innerGlow.Position = UDim2.new(0, 4, 0, 4)
    innerGlow.BackgroundTransparency = 1
    innerGlow.Parent = Player
    local innerGlowStroke = Instance.new("UIStroke", innerGlow)
    innerGlowStroke.Color = Color3.fromRGB(255, 120, 0)
    innerGlowStroke.Thickness = 2
    innerGlowStroke.Transparency = 0.9

    -- Top (cover + info)
    local top = Instance.new("Frame")
    top.Size = UDim2.new(1, 0, 0, 46)
    top.Position = UDim2.new(0, 0, 0, 10)
    top.BackgroundTransparency = 1
    top.Parent = Player

    -- Cover (46x46 com gradiente)
    local cover = Instance.new("Frame")
    cover.Size = UDim2.new(0, 46, 0, 46)
    cover.Position = UDim2.new(0, 10, 0, 0)
    cover.BorderSizePixel = 0
    cover.Parent = top
    local coverCorner = Instance.new("UICorner", cover)
    coverCorner.CornerRadius = UDim.new(0, 14)

    local coverGradient = Instance.new("UIGradient", cover)
    coverGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 123, 0)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 176, 0))
    })

    local coverEmoji = Instance.new("TextLabel")
    coverEmoji.Size = UDim2.new(1, 0, 1, 0)
    coverEmoji.BackgroundTransparency = 1
    coverEmoji.Text = "🎵"
    coverEmoji.TextColor3 = Color3.fromRGB(255, 255, 255)
    coverEmoji.Font = Enum.Font.SourceSansBold
    coverEmoji.TextSize = 18
    coverEmoji.Parent = cover

    -- Informações
    local info = Instance.new("Frame")
    info.Size = UDim2.new(1, -66, 1, 0)
    info.Position = UDim2.new(0, 56, 0, 0)
    info.BackgroundTransparency = 1
    info.Parent = top

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 20)
    title.Position = UDim2.new(0, 0, 0, 2)
    title.BackgroundTransparency = 1
    title.Text = "♧NERO MUSIC♡"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 11
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.TextTruncate = Enum.TextTruncate.AtEnd
    title.Parent = info

    local artist = Instance.new("TextLabel")
    artist.Size = UDim2.new(1, 0, 0, 16)
    artist.Position = UDim2.new(0, 0, 0, 24)
    artist.BackgroundTransparency = 1
    artist.Text = "for you"
    artist.TextColor3 = Color3.fromRGB(143, 143, 143)
    artist.Font = Enum.Font.Gotham
    artist.TextSize = 8
    artist.TextXAlignment = Enum.TextXAlignment.Left
    artist.TextTruncate = Enum.TextTruncate.AtEnd
    artist.Parent = info

    -- Barra de progresso
    local bar = Instance.new("Frame")
    bar.Size = UDim2.new(1, -20, 0, 3)
    bar.Position = UDim2.new(0, 10, 0, 64)
    bar.BackgroundColor3 = Color3.fromRGB(24, 24, 24)
    bar.BorderSizePixel = 0
    bar.Parent = Player
    local barCorner = Instance.new("UICorner", bar)
    barCorner.CornerRadius = UDim.new(0, 50)

    -- Barra de progresso (a que vai andar)
    local progress = Instance.new("Frame")
    progress.Size = UDim2.new(0, 0, 1, 0)  -- começa em 0%
    progress.BackgroundColor3 = Color3.fromRGB(255, 123, 0)
    progress.BorderSizePixel = 0
    progress.Parent = bar
    local progressCorner = Instance.new("UICorner", progress)
    progressCorner.CornerRadius = UDim.new(0, 50)

    -- Sombra da barra de progresso
    local progressShadow = Instance.new("Frame")
    progressShadow.Size = UDim2.new(0, 0, 1, 0)
    progressShadow.Position = UDim2.new(0, -4, 0, 0)
    progressShadow.BackgroundColor3 = Color3.fromRGB(255, 123, 0)
    progressShadow.BackgroundTransparency = 0.5
    progressShadow.BorderSizePixel = 0
    progressShadow.Parent = bar
    progressShadow.ZIndex = -1
    local shadowCorner = Instance.new("UICorner", progressShadow)
    shadowCorner.CornerRadius = UDim.new(0, 50)

    -- Controles (exatamente os mesmos caracteres do HTML)
    local controls = Instance.new("Frame")
    controls.Size = UDim2.new(1, 0, 0, 20)
    controls.Position = UDim2.new(0, 0, 0, 72)
    controls.BackgroundTransparency = 1
    controls.Parent = Player

    -- Botão Anterior
    local prevBtn = Instance.new("TextButton")
    prevBtn.Size = UDim2.new(0, 20, 0, 20)
    prevBtn.Position = UDim2.new(0.5, -48, 0, 0)
    prevBtn.BackgroundTransparency = 1
    prevBtn.Text = "《《"
    prevBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    prevBtn.Font = Enum.Font.GothamBold
    prevBtn.TextSize = 15
    prevBtn.Parent = controls

    -- Botão Play/Pause (fixo "▶")
    local playBtn = Instance.new("TextButton")
    playBtn.Size = UDim2.new(0, 20, 0, 20)
    playBtn.Position = UDim2.new(0.5, -10, 0, 0)
    playBtn.BackgroundTransparency = 1
    playBtn.Text = "▶"
    playBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    playBtn.Font = Enum.Font.GothamBold
    playBtn.TextSize = 15
    playBtn.Parent = controls

    -- Botão Próximo
    local nextBtn = Instance.new("TextButton")
    nextBtn.Size = UDim2.new(0, 20, 0, 20)
    nextBtn.Position = UDim2.new(0.5, 28, 0, 0)
    nextBtn.BackgroundTransparency = 1
    nextBtn.Text = "》》"
    nextBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    nextBtn.Font = Enum.Font.GothamBold
    nextBtn.TextSize = 15
    nextBtn.Parent = controls

    -- X para fechar
    local closeBtn = Instance.new("TextButton")
    closeBtn.Size = UDim2.new(0, 16, 0, 16)
    closeBtn.Position = UDim2.new(1, -20, 0, 6)
    closeBtn.BackgroundTransparency = 1
    closeBtn.Text = "✙"
    closeBtn.TextColor3 = Color3.fromRGB(180, 180, 180)
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 10
    closeBtn.Parent = Player

    -- ==========================================================
    -- ANIMAÇÕES (pulse da borda e cover)
    -- ==========================================================
    local TweenService = game:GetService("TweenService")

    local function animatePulse()
        local t1 = TweenService:Create(borderStroke, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Color = Color3.fromRGB(255, 149, 0)
        })
        local t2 = TweenService:Create(borderStroke, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Color = Color3.fromRGB(255, 123, 0)
        })
        local g1 = TweenService:Create(glowStroke, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Transparency = 0.3
        })
        local g2 = TweenService:Create(glowStroke, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Transparency = 0.7
        })
        local c1 = TweenService:Create(cover, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Size = UDim2.new(0, 48, 0, 48)
        })
        local c2 = TweenService:Create(cover, TweenInfo.new(0.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
            Size = UDim2.new(0, 46, 0, 46)
        })

        task.spawn(function()
            while MusicGui.Enabled do
                t1:Play(); g1:Play(); c1:Play()
                task.wait(0.6)
                t2:Play(); g2:Play(); c2:Play()
                task.wait(0.6)
            end
        end)
    end

    MusicGui:GetPropertyChangedSignal("Enabled"):Connect(function()
        if MusicGui.Enabled then
            animatePulse()
        end
    end)

    -- ==========================================================
    -- LÓGICA DO PLAYER (COM BARRA DE PROGRESSO)
    -- ==========================================================
    local RunService = game:GetService("RunService")
    local currentSound = nil
    local currentIndex = 1
    local isPlaying = false
    local progressConnection = nil

    -- Atualiza a barra de progresso
    local function UpdateProgress()
        if currentSound and currentSound.IsLoaded and currentSound.TimeLength > 0 then
            local progressPercent = math.clamp(currentSound.TimePosition / currentSound.TimeLength, 0, 1)
            progress.Size = UDim2.new(progressPercent, 0, 1, 0)
            progressShadow.Size = UDim2.new(progressPercent, 8, 1, 0)
            progressShadow.Position = UDim2.new(0, -4, 0, 0)
        else
            progress.Size = UDim2.new(0, 0, 1, 0)
            progressShadow.Size = UDim2.new(0, 0, 1, 0)
        end
    end

    -- Inicia a atualização contínua
    local function StartProgressUpdate()
        if progressConnection then progressConnection:Disconnect() end
        progressConnection = RunService.Heartbeat:Connect(function()
            if isPlaying and currentSound and currentSound.IsPlaying then
                UpdateProgress()
            end
        end)
    end

    -- Para a atualização (quando pausa ou para)
    local function StopProgressUpdate()
        if progressConnection then
            progressConnection:Disconnect()
            progressConnection = nil
        end
    end

    local function PlaySong(index)
        if currentSound then 
            currentSound:Stop() 
            currentSound:Destroy() 
        end
        if #musicList == 0 then return end
        if index < 1 then index = #musicList end
        if index > #musicList then index = 1 end
        currentIndex = index

        currentSound = Instance.new("Sound")
        currentSound.SoundId = "rbxassetid://" .. musicList[currentIndex]
        currentSound.Volume = 1.5
        currentSound.Looped = false
        currentSound.Parent = game:GetService("SoundService")
        currentSound:Play()
        isPlaying = true

        -- Aguarda o som carregar para pegar a duração
        currentSound.Loaded:Connect(function()
            UpdateProgress()
        end)

        -- Quando a música termina, volta a barra a 0 e vai para a próxima
        currentSound.Ended:Connect(function()
            progress.Size = UDim2.new(0, 0, 1, 0)
            progressShadow.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex + 1)
        end)

        StartProgressUpdate()
    end

    local function TogglePlay()
        if #musicList == 0 then return end
        if isPlaying then
            if currentSound then currentSound:Pause() end
            isPlaying = false
            StopProgressUpdate()
        else
            if currentSound then 
                currentSound:Resume()
                isPlaying = true
                StartProgressUpdate()
            else
                PlaySong(currentIndex)
            end
        end
        -- O ícone do play NÃO MUDA – permanece "▶"
    end

    -- ==========================================================
    -- EVENTOS DOS BOTÕES
    -- ==========================================================
    playBtn.MouseButton1Click:Connect(TogglePlay)
    nextBtn.MouseButton1Click:Connect(function()
        if #musicList > 0 then 
            progress.Size = UDim2.new(0, 0, 1, 0)
            progressShadow.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex + 1) 
        end
    end)
    prevBtn.MouseButton1Click:Connect(function()
        if #musicList > 0 then 
            progress.Size = UDim2.new(0, 0, 1, 0)
            progressShadow.Size = UDim2.new(0, 0, 1, 0)
            PlaySong(currentIndex - 1) 
        end
    end)

    -- Fechar – NÃO PARA A MÚSICA (mas a barra continua atualizando em segundo plano)
    closeBtn.MouseButton1Click:Connect(function()
        MusicGui.Enabled = false
        if not Main.Visible then NFContainer.Visible = true end
    end)

    -- ==========================================================
    -- ABRIR (SEGURAR N POR 3s)
    -- ==========================================================
    local function OpenMusicPlayer()
        MusicGui.Enabled = true
        if Main then Main.Visible = false end
        if NFContainer then NFContainer.Visible = false end
        if #musicList > 0 and not currentSound then
            currentIndex = math.random(1, #musicList)
            PlaySong(currentIndex)
        end
    end

    local NFBtn = NFContainer and NFContainer:FindFirstChild("TextButton")
    if NFBtn then
        local holdTimer = nil
        local isHeld = false

        NFBtn.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isHeld = true
                holdTimer = task.delay(3, function()
                    if isHeld and not MusicGui.Enabled then
                        OpenMusicPlayer()
                    end
                end)
            end
        end)

        NFBtn.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                isHeld = false
                if holdTimer then task.cancel(holdTimer); holdTimer = nil end
            end
        end)
    end

    -- ==========================================================
    -- ARRASTÁVEL
    -- ==========================================================
    local dragging = false
    local dragInput, dragStart, startPos

    Player.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = Player.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    Player.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            Player.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)

    print("🎵 [NERO] Music Player v7 carregado! Barra de progresso animada! Segure N por 3s.")
end) -- [[ NERO HUB: Módulo de Câmera (Mobile / Bypass) ]] --
task.spawn(function()
    if not game:IsLoaded() then game.Loaded:Wait() end

    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local TweenService = game:GetService("TweenService")
    local UserInputService = game:GetService("UserInputService")
    local CoreGui = game:GetService("CoreGui")

    local player = Players.LocalPlayer
    local camera = workspace.CurrentCamera

    -- Configurações Visuais do Nero
    local neroBlack = Color3.fromRGB(10, 10, 10)
    local neroOrange = Color3.fromRGB(255, 100, 0)

    -- Criando a Interface
    local neroGui = Instance.new("ScreenGui")
    neroGui.Name = "NeroCameraModule"
    neroGui.ResetOnSpawn = false
    neroGui.IgnoreGuiInset = true
    neroGui.ZIndexBehavior = Enum.ZIndexBehavior.Global

    -- Blindagem para executores Mobile (Impede crash se o CoreGui bloquear)
    local function safeParent(gui)
        if gethui then
            local success, res = pcall(gethui)
            if success and res then
                gui.Parent = res
                return
            end
        end
        
        local success = pcall(function()
            gui.Parent = CoreGui
        end)
        if success then return end
        
        pcall(function()
            gui.Parent = player:WaitForChild("PlayerGui")
        end)
    end

    safeParent(neroGui)

    local btn = Instance.new("TextButton")
    btn.Name = "NeroBtn"
    btn.Size = UDim2.new(0, 20, 0, 20)
    btn.Position = UDim2.new(0.5, -10, 0, 2) 
    btn.BackgroundColor3 = neroBlack
    btn.Text = "¤"
    btn.TextColor3 = neroOrange
    btn.TextSize = 14
    btn.Font = Enum.Font.Code
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    btn.Parent = neroGui

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Color = neroOrange
    stroke.Thickness = 1.5
    stroke.Parent = btn

    -- Animação Pulsante da Borda (Laranja)
    local pulseInfo = TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    local pulseTween = TweenService:Create(stroke, pulseInfo, {Thickness = 2.5, Transparency = 0.4})
    pulseTween:Play()

    -- Lógica de Inatividade (Efeito Fantasma 10% visível = 0.9 de Transparência)
    local fadeDelay = 2
    local lastInteractionTime = tick()
    local fadeTweenInfo = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

    local function updateVisibility(active)
        local targetTrans = active and 0 or 0.9
        TweenService:Create(btn, fadeTweenInfo, {BackgroundTransparency = targetTrans, TextTransparency = targetTrans}):Play()
        TweenService:Create(stroke, fadeTweenInfo, {Transparency = active and 0 or 0.9}):Play()
    end

    RunService.Heartbeat:Connect(function()
        if tick() - lastInteractionTime >= fadeDelay then
            if btn.BackgroundTransparency < 0.8 then 
                updateVisibility(false)
            end
        end
    end)

    -- Variáveis de Estado
    local cameraFixed = false
    local thirdPersonBypass = false
    local bypassConnection
    local fixedConnection

    -- Função: Burlar Câmera (3ª Pessoa Forçada)
    local function setThirdPersonBypass(state)
        thirdPersonBypass = state
        if bypassConnection then bypassConnection:Disconnect() end
        
        if state then
            camera.CameraType = Enum.CameraType.Scriptable
            
            bypassConnection = RunService.RenderStepped:Connect(function()
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChild("Head") then
                    local hrp = char.HumanoidRootPart
                    local camOffset = CFrame.new(0, 2, 10) 
                    camera.CFrame = hrp.CFrame * camOffset
                    
                    if cameraFixed then
                        UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
                    else
                        UserInputService.MouseBehavior = Enum.MouseBehavior.Default
                    end
                end
            end)
        else
            camera.CameraType = Enum.CameraType.Custom
            UserInputService.MouseBehavior = Enum.MouseBehavior.Default
        end
    end

    -- Função: Câmera Fixa (1 Toque)
    local function setFixedCamera(state)
        cameraFixed = state
        if fixedConnection then fixedConnection:Disconnect() end
        
        if state and not thirdPersonBypass then
            fixedConnection = RunService.RenderStepped:Connect(function()
                UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    local hrp = char.HumanoidRootPart
                    hrp.CFrame = CFrame.new(hrp.Position, hrp.Position + camera.CFrame.LookVector * Vector3.new(1,0,1))
                end
            end)
        else
            UserInputService.MouseBehavior = Enum.MouseBehavior.Default
        end
    end

    -- Lógica de Toques 
    local tapCount = 0
    local doubleTapTime = 0.3 

    btn.MouseButton1Click:Connect(function()
        lastInteractionTime = tick()
        updateVisibility(true) 
        
        tapCount = tapCount + 1
        
        if tapCount == 1 then
            task.delay(doubleTapTime + 0.05, function()
                if tapCount == 1 then
                    tapCount = 0
                    setFixedCamera(not cameraFixed)
                end
            end)
        elseif tapCount == 2 then
            tapCount = 0
            setThirdPersonBypass(not thirdPersonBypass)
            
            if not thirdPersonBypass then
                setFixedCamera(cameraFixed)
            end
        end
    end)
end)
