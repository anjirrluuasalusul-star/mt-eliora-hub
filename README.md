-- ANTI KICK
local mt = getrawmetatable(game)
setreadonly(mt,false)
local old = mt.__namecall

mt.__namecall = newcclosure(function(self,...)
    local args = {...}
    local method = getnamecallmethod()
    if tostring(method) == "Kick" then
        return warn("Anti Kick Triggered")
    end
    return old(self,...)
end)

-- ANTI AFK
local VirtualUser = game:GetService("VirtualUser")
game:GetService("Players").LocalPlayer.Idled:Connect(function()
    VirtualUser:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    task.wait(1)
    VirtualUser:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

-- RAYFIELD
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Auto Gunung Hub",
    LoadingTitle = "Auto Summit",
    LoadingSubtitle = "by Rey Cool",
    ConfigurationSaving = {
        Enabled = false
    },
    KeySystem = true,
    KeySettings = {
        Title = "Auto Gunung Hub",
        Subtitle = "Key System",
        Note = "Masukkan Key Untuk Menggunakan Script",
        FileName = "AutoGunungKey",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"REYCOOL123"}
    }
})

-- TAB
local MainTab = Window:CreateTab("Main",4483362458)
local TeleportTab = Window:CreateTab("Teleport",4483362458)
local PlayerTab = Window:CreateTab("Player",4483362458)
local MovementTab = Window:CreateTab("Movement",4483362458)
local AnimationTab = Window:CreateTab("Animation",4483362458)
local TrollTab = Window:CreateTab("Troll",4483362458)

local player = game.Players.LocalPlayer
local delayTeleport = 2
local auto = false

local locations = {
Vector3.new(2352.06,30.08,-65.24),
Vector3.new(19784.9,1528.38,-16428.92),
Vector3.new(19834.6,1527.67,-16520.45)
}

local function getHRP()
    local char = player.Character or player.CharacterAdded:Wait()
    return char:WaitForChild("HumanoidRootPart")
end

-- MAIN
MainTab:CreateToggle({
    Name = "Auto Summit",
    CurrentValue = false,
    Callback = function(Value)
        auto = Value
        if auto then
            task.spawn(function()
                while auto do
                    local hrp = getHRP()
                    for i,v in ipairs(locations) do
                        if not auto then break end
                        hrp.CFrame = CFrame.new(v)
                        task.wait(delayTeleport)
                    end
                end
            end)
        end
    end
})

MainTab:CreateSlider({
    Name = "Teleport Delay",
    Range = {1,5},
    Increment = 0.1,
    Suffix = "sec",
    CurrentValue = 2,
    Callback = function(Value)
        delayTeleport = Value
    end
})

-- TELEPORT
TeleportTab:CreateButton({
    Name = "Teleport Start",
    Callback = function()
        getHRP().CFrame = CFrame.new(2352.06,30.08,-65.24)
    end
})

TeleportTab:CreateButton({
    Name = "Teleport Mid",
    Callback = function()
        getHRP().CFrame = CFrame.new(19784.9,1528.38,-16428.92)
    end
})

TeleportTab:CreateButton({
    Name = "Teleport Summit",
    Callback = function()
        getHRP().CFrame = CFrame.new(19834.6,1527.67,-16520.45)
    end
})

-- PLAYER
PlayerTab:CreateSlider({
    Name = "WalkSpeed",
    Range = {16,200},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(Value)
        local char = player.Character
        if char then
            char:FindFirstChildOfClass("Humanoid").WalkSpeed = Value
        end
    end
})

PlayerTab:CreateSlider({
    Name = "JumpPower",
    Range = {50,200},
    Increment = 1,
    CurrentValue = 50,
    Callback = function(Value)
        local char = player.Character
        if char then
            char:FindFirstChildOfClass("Humanoid").JumpPower = Value
        end
    end
})

-- MOVEMENT
MovementTab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Callback = function(Value)
        _G.InfJump = Value
    end
})

game:GetService("UserInputService").JumpRequest:Connect(function()
    if _G.InfJump then
        local char = player.Character
        if char then
            char:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end
end)

-- ANIMATION
AnimationTab:CreateButton({
    Name = "Dance",
    Callback = function()
        local anim = Instance.new("Animation")
        anim.AnimationId = "rbxassetid://507771019"
        local track = player.Character:FindFirstChildOfClass("Humanoid"):LoadAnimation(anim)
        track:Play()
    end
})

-- TROLL
TrollTab:CreateButton({
    Name = "Spin Character",
    Callback = function()
        local hrp = getHRP()
        while task.wait() do
            hrp.CFrame = hrp.CFrame * CFrame.Angles(0,math.rad(30),0)
        end
    end
})
