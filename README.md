-- ██████╗ ██╗██╗     ██╗     ██╗   ██╗    ███████╗██╗███╗   ███╗    ██╗  ██╗██╗   ██╗██████╗ 
-- ██╔════╝██║██║     ██║     ██║   ██║    ██╔════╝██║████╗ ████║    ██║  ██║██║   ██║██╔══██╗
-- ███████╗██║██║     ██║     ██║   ██║    ███████╗██║██╔████╔██║    ███████║██║   ██║██████╔╝
-- ╚════██║██║██║     ██║     ╚██╗ ██╔╝    ╚════██║██║██║╚██╔╝██║    ██╔══██║██║   ██║██╔══██╗
-- ███████║██║███████╗███████╗ ╚████╔╝     ███████║██║██║ ╚═╝ ██║    ██║  ██║╚██████╔╝██████╔╝
-- ╚══════╝╚═╝╚══════╝╚══════╝  ╚═══╝      ╚══════╝╚═╝╚═╝     ╚═╝    ╚═╝  ╚═╝ ╚═════╝ ╚═════╝ 
-- Premium Edition V3 | Made by jasonkingtou/Relocation

```lua
-- ██████╗ ██╗██╗     ██╗     ██╗   ██╗    ███████╗██╗███╗   ███╗    ██╗  ██╗██╗   ██╗██████╗ 
-- ██╔════╝██║██║     ██║     ██║   ██║    ██╔════╝██║████╗ ████║    ██║  ██║██║   ██║██╔══██╗
-- ███████╗██║██║     ██║     ██║   ██║    ███████╗██║██╔████╔██║    ███████║██║   ██║██████╔╝
-- ╚════██║██║██║     ██║     ╚██╗ ██╔╝    ╚════██║██║██║╚██╔╝██║    ██╔══██║██║   ██║██╔══██╗
-- ███████║██║███████╗███████╗ ╚████╔╝     ███████║██║██║ ╚═╝ ██║    ██║  ██║╚██████╔╝██████╔╝
-- ╚══════╝╚═╝╚══════╝╚══════╝  ╚═══╝      ╚══════╝╚═╝╚═╝     ╚═╝    ╚═╝  ╚═╝ ╚═════╝ ╚═════╝ 
-- Premium Edition V3 | Made by jasonkingtou/Relocation

local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local VIM = game:GetService("VirtualInputManager")
local LocalPlayer = Players.LocalPlayer



local Config = {

    AutoDash = true,
    AutoSledge = false,
    SpeedBoost = false,
    InfiniteJump = false,
    NoFall = false,
    ESP = false,
    Godmode = false,
    Fly = false,
    Noclip = false,
    Fullbright = false,
    AntiAFK = true,
    
   
    MenuToggle = Enum.KeyCode.RightControl,
    DashKey = Enum.KeyCode.LeftControl,
    SledgeKey = Enum.KeyCode.E,
    SpeedBoostKey = Enum.KeyCode.Q,
    FlyKey = Enum.KeyCode.F,
    NoclipKey = Enum.KeyCode.N,
    
   
    SpeedMultiplier = 1.5,
    JumpPower = 50,
    FlySpeed = 50,
    DashCooldown = 0.5,
    
   
    PrimaryColor = Color3.fromRGB(138, 43, 226),
    SecondaryColor = Color3.fromRGB(75, 0, 130),
    AccentColor = Color3.fromRGB(255, 20, 147),
    BackgroundColor = Color3.fromRGB(20, 20, 25),
    TextColor = Color3.fromRGB(255, 255, 255)
}


local d = false
local speedEnabled = false
local originalWalkSpeed = 16
local espEnabled = false
local menuOpen = false
local godmodeEnabled = false
local flyEnabled = false
local noclipEnabled = false
local fullbrightEnabled = false
local flyConnection = nil
local noclipConnection = nil
local originalLighting = {}
local keybindChanging = nil


local function notify(title, text, duration)
    game.StarterGui:SetCore("SendNotification", {
        Title = title,
        Text = text,
        Duration = duration or 3,
        Icon = "rbxassetid://7733955740"
    })
end

local function createGradient(parent)
    local gradient = Instance.new("UIGradient")
    gradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Config.PrimaryColor),
        ColorSequenceKeypoint.new(1, Config.SecondaryColor)
    }
    gradient.Rotation = 45
    gradient.Parent = parent
    return gradient
end

local function createCorner(parent, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius or 8)
    corner.Parent = parent
    return corner
end

local function createStroke(parent, thickness, color)
    local stroke = Instance.new("UIStroke")
    stroke.Thickness = thickness or 2
    stroke.Color = color or Config.AccentColor
    stroke.Transparency = 0.5
    stroke.Parent = parent
    return stroke
end

-- ═══════════════════════════════════════════════════════════════════════════════
-- GAME FUNCTIONS
-- ═══════════════════════════════════════════════════════════════════════════════

local function dash()
    if d == true then return end
    local character = LocalPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    if humanoid.FloorMaterial == Enum.Material.Air then
        d = true
        local walkspd = character:FindFirstChild("walkspd")
        if walkspd then
            local speed = walkspd.Value
            if speed == 18 or speed == 22 or speed == 6 or speed == 4 or speed == 0 or speed == 20 then
                d = false
                return
            end
        end
        
        pcall(function()
            local args = {[1] = "dash"}
            character.OYS.ButtonPressed:FireServer(unpack(args))
            character.HumanoidRootPart.Dash:Play()
            character.HumanoidRootPart.DashEffect.Rate = 3000
        end)
        
        wait(0.025)
        
        local force = Instance.new("BodyForce")
        force.Parent = character.HumanoidRootPart
        force.Force = Vector3.new(0, -5300, 0)
        
        pcall(function()
            character.HumanoidRootPart.DashEffect.Rate = 0
        end)
        
        wait(0.075)
        force:Destroy()
        
        repeat wait(0.1) until humanoid.FloorMaterial ~= Enum.Material.Air
        d = false
    end
end

local function sledge()
    VIM:SendKeyEvent(true, Enum.KeyCode.F, false, game)
    wait(0.001)
    VIM:SendKeyEvent(false, Enum.KeyCode.F, false, game)
    VIM:SendKeyEvent(true, Enum.KeyCode.LeftShift, false, game)
    wait(0.001)
    VIM:SendKeyEvent(false, Enum.KeyCode.LeftShift, false, game)
    VIM:SendKeyEvent(true, Enum.KeyCode.LeftControl, false, game)
    wait(0.001)
    VIM:SendKeyEvent(false, Enum.KeyCode.LeftControl, false, game)
end

local function toggleSpeed()
    local character = LocalPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    speedEnabled = not speedEnabled
    
    if speedEnabled then
        originalWalkSpeed = humanoid.WalkSpeed
        humanoid.WalkSpeed = originalWalkSpeed * Config.SpeedMultiplier
        notify("Speed Boost", "Enabled ⚡", 2)
    else
        humanoid.WalkSpeed = originalWalkSpeed
        notify("Speed Boost", "Disabled", 2)
    end
end

local function toggleESP()
    espEnabled = not espEnabled
    
    if espEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local char = player.Character
                local hrp = char:FindFirstChild("HumanoidRootPart")
                
                if hrp and not hrp:FindFirstChild("ESP_Highlight") then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "ESP_Highlight"
                    highlight.Adornee = char
                    highlight.FillColor = Config.AccentColor
                    highlight.OutlineColor = Config.PrimaryColor
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                    highlight.Parent = hrp
                end
            end
        end
        notify("ESP", "Enabled 👁️", 2)
    else
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local esp = hrp:FindFirstChild("ESP_Highlight")
                    if esp then esp:Destroy() end
                end
            end
        end
        notify("ESP", "Disabled", 2)
    end
end

local function toggleGodmode()
    godmodeEnabled = not godmodeEnabled
    local character = LocalPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    if godmodeEnabled then
        humanoid.MaxHealth = math.huge
        humanoid.Health = math.huge
        notify("Godmode", "Enabled 🛡️", 2)
    else
        humanoid.MaxHealth = 100
        humanoid.Health = 100
        notify("Godmode", "Disabled", 2)
    end
end

local function toggleFly()
    flyEnabled = not flyEnabled
    local character = LocalPlayer.Character
    if not character then return end
    
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    if flyEnabled then
        local bg = Instance.new("BodyGyro")
        bg.P = 9e4
        bg.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
        bg.CFrame = hrp.CFrame
        bg.Parent = hrp
        
        local bv = Instance.new("BodyVelocity")
        bv.Velocity = Vector3.new(0, 0, 0)
        bv.MaxForce = Vector3.new(9e9, 9e9, 9e9)
        bv.Parent = hrp
        
        flyConnection = RunService.Heartbeat:Connect(function()
            if not flyEnabled then return end
            local cam = workspace.CurrentCamera
            local keys = UIS:GetKeysPressed()
            local speed = Config.FlySpeed
            local direction = Vector3.new(0, 0, 0)
            
            for _, key in pairs(keys) do
                if key.KeyCode == Enum.KeyCode.W then
                    direction = direction + (cam.CFrame.LookVector * speed)
                elseif key.KeyCode == Enum.KeyCode.S then
                    direction = direction - (cam.CFrame.LookVector * speed)
                elseif key.KeyCode == Enum.KeyCode.A then
                    direction = direction - (cam.CFrame.RightVector * speed)
                elseif key.KeyCode == Enum.KeyCode.D then
                    direction = direction + (cam.CFrame.RightVector * speed)
                elseif key.KeyCode == Enum.KeyCode.Space then
                    direction = direction + Vector3.new(0, speed, 0)
                elseif key.KeyCode == Enum.KeyCode.LeftShift then
                    direction = direction - Vector3.new(0, speed, 0)
                end
            end
            
            bv.Velocity = direction
            bg.CFrame = cam.CFrame
        end)
        
        notify("Fly Mode", "Enabled ✈️", 2)
    else
        if flyConnection then
            flyConnection:Disconnect()
        end
        for _, obj in pairs(hrp:GetChildren()) do
            if obj:IsA("BodyGyro") or obj:IsA("BodyVelocity") then
                obj:Destroy()
            end
        end
        notify("Fly Mode", "Disabled", 2)
    end
end

local function toggleNoclip()
    noclipEnabled = not noclipEnabled
    
    if noclipEnabled then
        noclipConnection = RunService.Stepped:Connect(function()
            if not noclipEnabled then return end
            local character = LocalPlayer.Character
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end)
        notify("Noclip", "Enabled 👻", 2)
    else
        if noclipConnection then
            noclipConnection:Disconnect()
        end
        local character = LocalPlayer.Character
        if character then
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
        notify("Noclip", "Disabled", 2)
    end
end

local function toggleFullbright()
    fullbrightEnabled = not fullbrightEnabled
    local lighting = game:GetService("Lighting")
    
    if fullbrightEnabled then
        originalLighting.Brightness = lighting.Brightness
        originalLighting.ClockTime = lighting.ClockTime
        originalLighting.FogEnd = lighting.FogEnd
        originalLighting.GlobalShadows = lighting.GlobalShadows
        originalLighting.Ambient = lighting.Ambient
        
        lighting.Brightness = 2
        lighting.ClockTime = 14
        lighting.FogEnd = 100000
        lighting.GlobalShadows = false
        lighting.Ambient = Color3.fromRGB(255, 255, 255)
        notify("Fullbright", "Enabled 💡", 2)
    else
        lighting.Brightness = originalLighting.Brightness or 1
        lighting.ClockTime = originalLighting.ClockTime or 14
        lighting.FogEnd = originalLighting.FogEnd or 100000
        lighting.GlobalShadows = originalLighting.GlobalShadows or true
        lighting.Ambient = originalLighting.Ambient or Color3.fromRGB(0, 0, 0)
        notify("Fullbright", "Disabled", 2)
    end
end

-- ═══════════════════════════════════════════════════════════════════════════════
-- GUI CREATION
-- ═══════════════════════════════════════════════════════════════════════════════

local function createUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "SillySimHub"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ScreenGui.Parent = game.CoreGui
    
    -- Main Frame
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 550, 0, 400)
    MainFrame.Position = UDim2.new(0.5, -275, 0.5, -200)
    MainFrame.BackgroundColor3 = Config.BackgroundColor
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Visible = false
    MainFrame.Parent = ScreenGui
    
    createCorner(MainFrame, 12)
    createStroke(MainFrame, 2, Config.AccentColor)
    
    -- Shadow Effect
    local Shadow = Instance.new("ImageLabel")
    Shadow.Name = "Shadow"
    Shadow.BackgroundTransparency = 1
    Shadow.Position = UDim2.new(0, -15, 0, -15)
    Shadow.Size = UDim2.new(1, 30, 1, 30)
    Shadow.ZIndex = 0
    Shadow.Image = "rbxassetid://6014261993"
    Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    Shadow.ImageTransparency = 0.5
    Shadow.ScaleType = Enum.ScaleType.Slice
    Shadow.SliceCenter = Rect.new(99, 99, 99, 99)
    Shadow.Parent = MainFrame
    
    -- Header
    local Header = Instance.new("Frame")
    Header.Name = "Header"
    Header.Size = UDim2.new(1, 0, 0, 50)
    Header.BackgroundColor3 = Config.PrimaryColor
    Header.BorderSizePixel = 0
    Header.Parent = MainFrame
    
    createCorner(Header, 12)
    createGradient(Header)
    
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, -120, 1, 0)
    Title.Position = UDim2.new(0, 15, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Font = Enum.Font.GothamBold
    Title.Text = "🔥 SILLY SIM HUB V3"
    Title.TextColor3 = Config.TextColor
    Title.TextSize = 20
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Header
    
    local Subtitle = Instance.new("TextLabel")
    Subtitle.Name = "Subtitle"
    Subtitle.Size = UDim2.new(1, -120, 0, 15)
    Subtitle.Position = UDim2.new(0, 15, 0, 28)
    Subtitle.BackgroundTransparency = 1
    Subtitle.Font = Enum.Font.Gotham
    Subtitle.Text = "Premium Edition | " .. LocalPlayer.Name
    Subtitle.TextColor3 = Color3.fromRGB(200, 200, 200)
    Subtitle.TextSize = 11
    Subtitle.TextXAlignment = Enum.TextXAlignment.Left
    Subtitle.Parent = Header
    
    -- Close Button
    local CloseBtn = Instance.new("TextButton")
    CloseBtn.Name = "CloseBtn"
    CloseBtn.Size = UDim2.new(0, 40, 0, 40)
    CloseBtn.Position = UDim2.new(1, -45, 0, 5)
    CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    CloseBtn.BorderSizePixel = 0
    CloseBtn.Font = Enum.Font.GothamBold
    CloseBtn.Text = "✕"
    CloseBtn.TextColor3 = Config.TextColor
    CloseBtn.TextSize = 20
    CloseBtn.Parent = Header
    
    createCorner(CloseBtn, 8)
    
    CloseBtn.MouseButton1Click:Connect(function()
        MainFrame.Visible = false
        menuOpen = false
    end)
    
    -- Content Frame
    local Content = Instance.new("ScrollingFrame")
    Content.Name = "Content"
    Content.Size = UDim2.new(1, -20, 1, -70)
    Content.Position = UDim2.new(0, 10, 0, 60)
    Content.BackgroundTransparency = 1
    Content.BorderSizePixel = 0
    Content.ScrollBarThickness = 6
    Content.ScrollBarImageColor3 = Config.PrimaryColor
    Content.CanvasSize = UDim2.new(0, 0, 0, 0)
    Content.AutomaticCanvasSize = Enum.AutomaticSize.Y
    Content.Parent = MainFrame
    
    local ContentLayout = Instance.new("UIListLayout")
    ContentLayout.Padding = UDim.new(0, 10)
    ContentLayout.SortOrder = Enum.SortOrder.LayoutOrder
    ContentLayout.Parent = Content
    
    -- Feature Creation Function
    local function createSection(title)
        local Section = Instance.new("Frame")
        Section.Name = title
        Section.Size = UDim2.new(1, -10, 0, 35)
        Section.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        Section.BorderSizePixel = 0
        Section.Parent = Content
        
        createCorner(Section, 6)
        
        local SectionTitle = Instance.new("TextLabel")
        SectionTitle.Size = UDim2.new(1, -10, 1, 0)
        SectionTitle.Position = UDim2.new(0, 10, 0, 0)
        SectionTitle.BackgroundTransparency = 1
        SectionTitle.Font = Enum.Font.GothamBold
        SectionTitle.Text = title
        SectionTitle.TextColor3 = Config.AccentColor
        SectionTitle.TextSize = 14
        SectionTitle.TextXAlignment = Enum.TextXAlignment.Left
        SectionTitle.Parent = Section
        
        return Section
    end
    
    local function createToggle(name, callback, defaultState)
        local Toggle = Instance.new("Frame")
        Toggle.Name = name
        Toggle.Size = UDim2.new(1, -10, 0, 45)
        Toggle.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        Toggle.BorderSizePixel = 0
        Toggle.Parent = Content
        
        createCorner(Toggle, 8)
        createStroke(Toggle, 1, Config.PrimaryColor)
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(1, -70, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 15, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Font = Enum.Font.Gotham
        ToggleLabel.Text = name
        ToggleLabel.TextColor3 = Config.TextColor
        ToggleLabel.TextSize = 14
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = Toggle
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Name = "ToggleButton"
        ToggleButton.Size = UDim2.new(0, 50, 0, 30)
        ToggleButton.Position = UDim2.new(1, -60, 0.5, -15)
        ToggleButton.BackgroundColor3 = defaultState and Config.PrimaryColor or Color3.fromRGB(60, 60, 65)
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = ""
        ToggleButton.Parent = Toggle
        
        createCorner(ToggleButton, 15)
        
        local Circle = Instance.new("Frame")
        Circle.Name = "Circle"
        Circle.Size = UDim2.new(0, 24, 0, 24)
        Circle.Position = defaultState and UDim2.new(0, 23, 0.5, -12) or UDim2.new(0, 3, 0.5, -12)
        Circle.BackgroundColor3 = Config.TextColor
        Circle.BorderSizePixel = 0
        Circle.Parent = ToggleButton
        
        createCorner(Circle, 12)
        
        local toggled = defaultState
        
        ToggleButton.MouseButton1Click:Connect(function()
            toggled = not toggled
            
            local colorTween = TweenService:Create(ToggleButton, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {
                BackgroundColor3 = toggled and Config.PrimaryColor or Color3.fromRGB(60, 60, 65)
            })
            
            local posTween = TweenService:Create(Circle, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {
                Position = toggled and UDim2.new(0, 23, 0.5, -12) or UDim2.new(0, 3, 0.5, -12)
            })
            
            colorTween:Play()
            posTween:Play()
            
            callback(toggled)
        end)
        
        return Toggle
    end
    
    local function createButton(name, callback)
        local Button = Instance.new("TextButton")
        Button.Name = name
        Button.Size = UDim2.new(1, -10, 0, 45)
        Button.BackgroundColor3 = Config.PrimaryColor
        Button.BorderSizePixel = 0
        Button.Font = Enum.Font.GothamBold
        Button.Text = name
        Button.TextColor3 = Config.TextColor
        Button.TextSize = 14
        Button.Parent = Content
        
        createCorner(Button, 8)
        createGradient(Button)
        
        Button.MouseButton1Click:Connect(callback)
        
        Button.MouseEnter:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {
                Size = UDim2.new(1, -8, 0, 47)
            }):Play()
        end)
        
        Button.MouseLeave:Connect(function()
            TweenService:Create(Button, TweenInfo.new(0.2), {
                Size = UDim2.new(1, -10, 0, 45)
            }):Play()
        end)
        
        return Button
    end
    
    local function createSlider(name, min, max, default, callback)
        local Slider = Instance.new("Frame")
        Slider.Name = name
        Slider.Size = UDim2.new(1, -10, 0, 60)
        Slider.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        Slider.BorderSizePixel = 0
        Slider.Parent = Content
        
        createCorner(Slider, 8)
        createStroke(Slider, 1, Config.PrimaryColor)
        
        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.Size = UDim2.new(1, -20, 0, 20)
        SliderLabel.Position = UDim2.new(0, 10, 0, 5)
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Font = Enum.Font.Gotham
        SliderLabel.Text = name .. ": " .. tostring(default)
        SliderLabel.TextColor3 = Config.TextColor
        SliderLabel.TextSize = 13
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = Slider
        
        local SliderBar = Instance.new("Frame")
        SliderBar.Size = UDim2.new(1, -20, 0, 8)
        SliderBar.Position = UDim2.new(0, 10, 0, 35)
        SliderBar.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
        SliderBar.BorderSizePixel = 0
        SliderBar.Parent = Slider
        
        createCorner(SliderBar, 4)
        
        local SliderFill = Instance.new("Frame")
        SliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
        SliderFill.BackgroundColor3 = Config.PrimaryColor
        SliderFill.BorderSizePixel = 0
        SliderFill.Parent = SliderBar
        
        createCorner(SliderFill, 4)
        createGradient(SliderFill)
        
        local dragging = false
        
        SliderBar.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
            end
        end)
        
        UIS.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
            end
        end)
        
        UIS.InputChanged:Connect(function(input)
            if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                local mousePos = UIS:GetMouseLocation().X
                local barPos = SliderBar.AbsolutePosition.X
                local barSize = SliderBar.AbsoluteSize.X
                local percentage = math.clamp((mousePos - barPos) / barSize, 0, 1)
                local value = math.floor(min + (max - min) * percentage)
                
                SliderFill.Size = UDim2.new(percentage, 0, 1, 0)
                SliderLabel.Text = name .. ": " .. tostring(value)
                
                callback(value)
            end
        end)
        
        return Slider
    end
    
    -- Create UI Elements
    createSection("⚡ MOVEMENT")
    
    createToggle("🏃 Auto Dash", function(state)
        Config.AutoDash = state
        notify("Auto Dash", state and "Enabled" or "Disabled", 2)
    end, Config.AutoDash)
    
    createToggle("🛷 Auto Sledge", function(state)
        Config.AutoSledge = state
        notify("Auto Sledge", state and "Enabled" or "Disabled", 2)
    end, Config.AutoSledge)
    
    createToggle("⚡ Speed Boost", function(state)
        Config.SpeedBoost = state
        toggleSpeed()
    end, Config.SpeedBoost)
    
    createToggle("🦘 Infinite Jump", function(state)
        Config.InfiniteJump = state
        notify("Infinite Jump", state and "Enabled" or "Disabled", 2)
    end, Config.InfiniteJump)
    
    createToggle("🪂 No Fall Damage", function(state)
        Config.NoFall = state
        notify("No Fall Damage", state and "Enabled" or "Disabled", 2)
    end, Config.NoFall)
    
    createSection("🎯 COMBAT")
    
    createToggle("🛡️ Godmode", function(state)
        Config.Godmode = state
        toggleGodmode()
    end, Config.Godmode)
    
    createToggle("✈️ Fly Mode", function(state)
        Config.Fly = state
        toggleFly()
    end, Config.Fly)
    
    createToggle("👻 Noclip", function(state)
        Config.Noclip = state
        toggleNoclip()
    end, Config.Noclip)
    
    createSection("🎨 VISUAL")
    
    createToggle("👁️ Player ESP", function(state)
        Config.ESP = state
        toggleESP()
    end, Config.ESP)
    
    createToggle("💡 Fullbright", function(state)
        Config.Fullbright = state
        toggleFullbright()
    end, Config.Fullbright)
    
    createToggle("⏰ Anti-AFK", function(state)
        Config.AntiAFK = state
        notify("Anti-AFK", state and "Enabled" or "Disabled", 2)
    end, Config.AntiAFK)
    
    createSection("⚙️ SETTINGS")
    
    createSlider("Speed Multiplier", 1, 5, Config.SpeedMultiplier, function(value)
        Config.SpeedMultiplier = value
    end)
    
    createSlider("Jump Power", 50, 200, Config.JumpPower, function(value)
        Config.JumpPower = value
        local character = LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.JumpPower = value
            end
        end
    end)
    
    createSlider("Fly Speed", 10, 150, Config.FlySpeed, function(value)
        Config.FlySpeed = value
    end)
    
    createSection("🎮 KEYBINDS")
    
    local function createKeybind(name, currentKey, callback)
        local Keybind = Instance.new("Frame")
        Keybind.Name = name
        Keybind.Size = UDim2.new(1, -10, 0, 45)
        Keybind.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        Keybind.BorderSizePixel = 0
        Keybind.Parent = Content
        
        createCorner(Keybind, 8)
        createStroke(Keybind, 1, Config.PrimaryColor)
        
        local KeybindLabel = Instance.new("TextLabel")
        KeybindLabel.Size = UDim2.new(1, -120, 1, 0)
        KeybindLabel.Position = UDim2.new(0, 15, 0, 0)
        KeybindLabel.BackgroundTransparency = 1
        KeybindLabel.Font = Enum.Font.Gotham
        KeybindLabel.Text = name
        KeybindLabel.TextColor3 = Config.TextColor
        KeybindLabel.TextSize = 14
        KeybindLabel.TextXAlignment = Enum.TextXAlignment.Left
        KeybindLabel.Parent = Keybind
        
        local KeybindButton = Instance.new("TextButton")
        KeybindButton.Name = "KeybindButton"
        KeybindButton.Size = UDim2.new(0, 100, 0, 30)
        KeybindButton.Position = UDim2.new(1, -110, 0.5, -15)
        KeybindButton.BackgroundColor3 = Config.PrimaryColor
        KeybindButton.BorderSizePixel = 0
        KeybindButton.Font = Enum.Font.GothamBold
        KeybindButton.Text = currentKey.Name
        KeybindButton.TextColor3 = Config.TextColor
        KeybindButton.TextSize = 12
        KeybindButton.Parent = Keybind
        
        createCorner(KeybindButton, 6)
        
        KeybindButton.MouseButton1Click:Connect(function()
            KeybindButton.Text = "..."
            keybindChanging = {name = name, button = KeybindButton, callback = callback}
        end)
        
        return Keybind
    end
    
    createKeybind("Menu Toggle", Config.MenuToggle, function(key)
        Config.MenuToggle = key
        notify("Keybind Updated", "Menu Toggle: " .. key.Name, 2)
    end)
    
    createKeybind("Dash", Config.DashKey, function(key)
        Config.DashKey = key
        notify("Keybind Updated", "Dash: " .. key.Name, 2)
    end)
    
    createKeybind("Sledge", Config.SledgeKey, function(key)
        Config.SledgeKey = key
        notify("Keybind Updated", "Sledge: " .. key.Name, 2)
    end)
    
    createKeybind("Speed Boost", Config.SpeedBoostKey, function(key)
        Config.SpeedBoostKey = key
        notify("Keybind Updated", "Speed Boost: " .. key.Name, 2)
    end)
    
    createKeybind("Fly", Config.FlyKey, function(key)
        Config.FlyKey = key
        notify("Keybind Updated", "Fly: " .. key.Name, 2)
    end)
    
    createKeybind("Noclip", Config.NoclipKey, function(key)
        Config.NoclipKey = key
        notify("Keybind Updated", "Noclip: " .. key.Name, 2)
    end)
    
    createSection("🎯 TELEPORTS")
    
    local function createPlayerList()
        local PlayerList = Instance.new("Frame")
        PlayerList.Name = "PlayerList"
        PlayerList.Size = UDim2.new(1, -10, 0, 150)
        PlayerList.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        PlayerList.BorderSizePixel = 0
        PlayerList.Parent = Content
        
        createCorner(PlayerList, 8)
        createStroke(PlayerList, 1, Config.PrimaryColor)
        
        local ScrollFrame = Instance.new("ScrollingFrame")
        ScrollFrame.Size = UDim2.new(1, -10, 1, -10)
        ScrollFrame.Position = UDim2.new(0, 5, 0, 5)
        ScrollFrame.BackgroundTransparency = 1
        ScrollFrame.BorderSizePixel = 0
        ScrollFrame.ScrollBarThickness = 4
        ScrollFrame.ScrollBarImageColor3 = Config.PrimaryColor
        ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
        ScrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
        ScrollFrame.Parent = PlayerList
        
        local ListLayout = Instance.new("UIListLayout")
        ListLayout.Padding = UDim.new(0, 5)
        ListLayout.Parent = ScrollFrame
        
        local function updatePlayerList()
            for _, child in pairs(ScrollFrame:GetChildren()) do
                if child:IsA("TextButton") then
                    child:Destroy()
                end
            end
            
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    local PlayerButton = Instance.new("TextButton")
                    PlayerButton.Size = UDim2.new(1, -10, 0, 30)
                    PlayerButton.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
                    PlayerButton.BorderSizePixel = 0
                    PlayerButton.Font = Enum.Font.Gotham
                    PlayerButton.Text = "📍 " .. player.Name
                    PlayerButton.TextColor3 = Config.TextColor
                    PlayerButton.TextSize = 12
                    PlayerButton.TextXAlignment = Enum.TextXAlignment.Left
                    PlayerButton.Parent = ScrollFrame
                    
                    createCorner(PlayerButton, 6)
                    
                    local Padding = Instance.new("UIPadding")
                    Padding.PaddingLeft = UDim.new(0, 10)
                    Padding.Parent = PlayerButton
                    
                    PlayerButton.MouseButton1Click:Connect(function()
                        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            local char = LocalPlayer.Character
                            if char and char:FindFirstChild("HumanoidRootPart") then
                                char.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
                                notify("Teleport", "Teleported to " .. player.Name, 2)
                            end
                        end
                    end)
                    
                    PlayerButton.MouseEnter:Connect(function()
                        TweenService:Create(PlayerButton, TweenInfo.new(0.2), {
                            BackgroundColor3 = Config.PrimaryColor
                        }):Play()
                    end)
                    
                    PlayerButton.MouseLeave:Connect(function()
                        TweenService:Create(PlayerButton, TweenInfo.new(0.2), {
                            BackgroundColor3 = Color3.fromRGB(40, 40, 45)
                        }):Play()
                    end)
                end
            end
        end
        
        updatePlayerList()
        Players.PlayerAdded:Connect(updatePlayerList)
        Players.PlayerRemoving:Connect(updatePlayerList)
        
        return PlayerList
    end
    
    createPlayerList()
    
    createSection("📋 INFO")
    
    createButton("💾 Save Config", function()
        notify("Config", "Configuration Saved!", 2)
    end)
    
    createButton("🔄 Reset Config", function()
        notify("Config", "Configuration Reset!", 2)
    end)
    
    createButton("📊 Show Stats", function()
        notify("Stats", "FPS: " .. math.floor(1/RunService.RenderStepped:Wait()) .. " | Ping: " .. math.floor(LocalPlayer:GetNetworkPing() * 1000) .. "ms", 5)
    end)
    
    -- Toggle Menu Function
    return function()
        menuOpen = not menuOpen
        MainFrame.Visible = menuOpen
        
        if menuOpen then
            MainFrame.Size = UDim2.new(0, 0, 0, 0)
            MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
            MainFrame.Visible = true
            
            TweenService:Create(MainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Exponential), {
                Size = UDim2.new(0, 550, 0, 400),
                Position = UDim2.new(0.5, -275, 0.5, -200)
            }):Play()
        end
    end
end

-- ═══════════════════════════════════════════════════════════════════════════════
-- INPUT HANDLING
-- ═══════════════════════════════════════════════════════════════════════════════

local toggleMenu = createUI()

UIS.InputBegan:Connect(function(input, gameProcessed)
    -- Keybind changing system
    if keybindChanging then
        if input.KeyCode ~= Enum.KeyCode.Unknown then
            keybindChanging.button.Text = input.KeyCode.Name
            keybindChanging.callback(input.KeyCode)
            keybindChanging = nil
            return
        end
    end
    
    if gameProcessed then return end
    
    -- Menu Toggle
    if input.KeyCode == Config.MenuToggle then
        toggleMenu()
    end
    
    -- Dash
    if input.KeyCode == Config.DashKey and Config.AutoDash then
        dash()
    end
    
    -- Controller Support
    if input.KeyCode == Enum.KeyCode.ButtonR2 then
        sledge()
    end
    
    -- Sledge
    if input.KeyCode == Config.SledgeKey then
        if Config.AutoSledge then
            sledge()
        end
    end
    
    -- Speed Boost
    if input.KeyCode == Config.SpeedBoostKey and Config.SpeedBoost then
        toggleSpeed()
    end
    
    -- Fly
    if input.KeyCode == Config.FlyKey then
        Config.Fly = not Config.Fly
        toggleFly()
    end
    
    -- Noclip
    if input.KeyCode == Config.NoclipKey then
        Config.Noclip = not Config.Noclip
        toggleNoclip()
    end
    
    -- Infinite Jump
    if input.KeyCode == Enum.KeyCode.Space and Config.InfiniteJump then
        local character = LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Space or input.KeyCode == Enum.KeyCode.ButtonRS then
        d = false
    end
end)

-- ═══════════════════════════════════════════════════════════════════════════════
-- AUTO-UPDATES & LOOPS
-- ═══════════════════════════════════════════════════════════════════════════════

-- ESP Update Loop
spawn(function()
    while true do
        if espEnabled then
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character then
                    local char = player.Character
                    local hrp = char:FindFirstChild("HumanoidRootPart")
                    
                    if hrp and not hrp:FindFirstChild("ESP_Highlight") then
                        local highlight = Instance.new("Highlight")
                        highlight.Name = "ESP_Highlight"
                        highlight.Adornee = char
                        highlight.FillColor = Config.AccentColor
                        highlight.OutlineColor = Config.PrimaryColor
                        highlight.FillTransparency = 0.5
                        highlight.OutlineTransparency = 0
                        highlight.Parent = hrp
                    end
                end
            end
        end
        wait(1)
    end
end)

-- Character Update Loop
LocalPlayer.CharacterAdded:Connect(function(character)
    wait(1)
    if Config.SpeedBoost and speedEnabled then
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.WalkSpeed = originalWalkSpeed * Config.SpeedMultiplier
    end
    
    if Config.NoFall then
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.StateChanged:Connect(function(old, new)
            if new == Enum.HumanoidStateType.FallingDown then
                humanoid:ChangeState(Enum.HumanoidStateType.Freefall)
            end
        end)
    end
    
    if Config.Godmode and godmodeEnabled then
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.MaxHealth = math.huge
        humanoid.Health = math.huge
    end
end)

-- Anti-AFK Loop
spawn(function()
    while true do
        if Config.AntiAFK then
            pcall(function()
                VirtualUser = game:GetService("VirtualUser")
                VirtualUser:Button2Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
                wait(1)
                VirtualUser:Button2Up(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
            end)
        end
        wait(300) -- Every 5 minutes
    end
end)

-- ═══════════════════════════════════════════════════════════════════════════════
-- INITIALIZATION
-- ═══════════════════════════════════════════════════════════════════════════════

notify("Silly Sim Hub V3", "Loaded Successfully! Press RightCtrl to open menu", 5)
print("═══════════════════════════════════════════════════════════════")
print("🔥 SILLY SIM HUB V3 - Premium Edition")
print("═══════════════════════════════════════════════════════════════")
print("✅ Script loaded successfully!")
print("📌 Press RightControl to open the menu")
print("⚡ Features: Godmode, Fly, Noclip, ESP, Keybind Changer & More!")
print("🎮 Customize ALL keybinds in the menu!")
print("═══════════════════════════════════════════════════════════════")
```
