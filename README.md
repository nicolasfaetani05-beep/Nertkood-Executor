--[[ 
  NERTKOOD EXECUTOR v6.0 - THE FULL BUILD
  Color Settings, FPS Boost, Search API, and Floating UI.
--]]

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)

-- 1. MAIN FRAME
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Visible = false

-- 2. SEARCH ENGINE (cloud)
local SearchBox = Instance.new("TextBox", MainFrame)
SearchBox.PlaceholderText = "Search ScriptBlox..."
SearchBox.Size = UDim2.new(0, 300, 0, 40)
SearchBox.Position = UDim2.new(0, 10, 0, 10)

local SearchBtn = Instance.new("TextButton", MainFrame)
SearchBtn.Text = "Search"
SearchBtn.Size = UDim2.new(0, 70, 0, 40)
SearchBtn.Position = UDim2.new(0, 320, 0, 10)

-- 3. FPS BOOST (CLIENT SIDE)
local FpsBtn = Instance.new("TextButton", MainFrame)
FpsBtn.Text = "Increase FPS"
FpsBtn.Size = UDim2.new(0, 380, 0, 40)
FpsBtn.Position = UDim2.new(0, 10, 0, 60)
FpsBtn.MouseButton1Click:Connect(function()
    for _, item in pairs(game:GetDescendants()) do
        if item:IsA("Decal") or item:IsA("Texture") then
            item:Destroy()
        end
    end
end)

-- 4. SETTINGS (COLOR CHANGER)
local ColorDisplay = Instance.new("Frame", MainFrame)
ColorDisplay.Size = UDim2.new(0, 380, 0, 40)
ColorDisplay.Position = UDim2.new(0, 10, 0, 110)
ColorDisplay.BackgroundColor3 = Color3.new(1, 0.5, 0) -- Orange Default

local ColorBtn = Instance.new("TextButton", MainFrame)
ColorBtn.Text = "Change Color (Purple/Blue/Green/Orange)"
ColorBtn.Size = UDim2.new(0, 380, 0, 40)
ColorBtn.Position = UDim2.new(0, 10, 0, 160)
ColorBtn.MouseButton1Click:Connect(function()
    -- Rotates colors
    local colors = {Color3.new(0.5, 0, 0.5), Color3.new(0, 0, 1), Color3.new(0, 1, 0), Color3.new(1, 0.5, 0)}
    FloatingButton.BackgroundColor3 = colors[math.random(1, #colors)]
end)

-- 5. EXIT & FLOATING BUTTON
local ExitBtn = Instance.new("TextButton", MainFrame)
ExitBtn.Text = "Exit"
ExitBtn.Size = UDim2.new(0, 380, 0, 40)
ExitBtn.Position = UDim2.new(0, 10, 0, 210)
ExitBtn.MouseButton1Click:Connect(function() MainFrame.Visible = false end)

local FloatingButton = Instance.new("ImageButton", ScreenGui)
FloatingButton.Size = UDim2.new(0, 50, 0, 50)
FloatingButton.Position = UDim2.new(0, 50, 0.5, 0)
FloatingButton.BackgroundColor3 = Color3.new(1, 0.5, 0)
Instance.new("UICorner", FloatingButton).CornerRadius = UDim.new(1, 0)

-- Dragging Logic
local dragging, dragStart, startPos
FloatingButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = FloatingButton.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging then
        local delta = input.Position - dragStart
        FloatingButton.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

UserInputService.InputEnded:Connect(function(input) dragging = false end)
FloatingButton.MouseButton1Click:Connect(function() MainFrame.Visible = not MainFrame.Visible end)
