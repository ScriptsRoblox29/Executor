-- Create ScreenGui
local gui = Instance.new("ScreenGui", game.CoreGui)
local tweenService = game:GetService("TweenService")
local userInputService = game:GetService("UserInputService")
local tweenService2 = game:GetService("TweenService")

-- Create Shadow
local shadow = Instance.new("Frame", gui)
shadow.Size = UDim2.new(0, 440, 0, 340)
shadow.Position = UDim2.new(0.5, -220, 0.5, -170)
shadow.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
shadow.BackgroundTransparency = 0.6
shadow.ZIndex = 0
Instance.new("UICorner", shadow).CornerRadius = UDim.new(0, 20)

-- Create Main Frame
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 420, 0, 320)
mainFrame.Position = UDim2.new(0.5, -210, 0.5, -160)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 50)
mainFrame.BackgroundTransparency = 0
mainFrame.ZIndex = 1
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 20)

-- Gradient
local gradient = Instance.new("UIGradient", mainFrame)
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 40, 120)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(60, 60, 180))
}
gradient.Rotation = 90

-- Stroke
local stroke = Instance.new("UIStroke", mainFrame)
stroke.Thickness = 1.8
stroke.Color = Color3.fromRGB(200, 200, 255)
stroke.Transparency = 0.7

-- Create ScrollingFrame
local scrollingFrame = Instance.new("ScrollingFrame", mainFrame)
scrollingFrame.Size = UDim2.new(1, -30, 0, 210)
scrollingFrame.Position = UDim2.new(0, 15, 0, 15)
scrollingFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 40)
scrollingFrame.ScrollBarThickness = 5
scrollingFrame.CanvasSize = UDim2.new(0, 0, 2, 0)
scrollingFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
scrollingFrame.ClipsDescendants = true
scrollingFrame.ScrollBarImageColor3 = Color3.fromRGB(200, 200, 255)
Instance.new("UICorner", scrollingFrame).CornerRadius = UDim.new(0, 12)

-- Create TextBox
local textBox = Instance.new("TextBox", scrollingFrame)
textBox.Size = UDim2.new(1, -10, 0, 200)
textBox.Position = UDim2.new(0, 5, 0, 5)
textBox.BackgroundTransparency = 1
textBox.TextColor3 = Color3.new(1, 1, 1)
textBox.TextXAlignment = Enum.TextXAlignment.Left
textBox.TextYAlignment = Enum.TextYAlignment.Top
textBox.Font = Enum.Font.Gotham
textBox.TextSize = 17
textBox.Text = ""
textBox.ClearTextOnFocus = false
textBox.MultiLine = true
textBox.TextWrapped = false
textBox.AutomaticSize = Enum.AutomaticSize.Y

-- Create Button Function
local function createButton(name, text, position, size, color)
    local button = Instance.new("TextButton", mainFrame)
    button.Name = name
    button.Size = size
    button.Position = position
    button.BackgroundColor3 = color
    button.Text = text
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 18
    Instance.new("UICorner", button).CornerRadius = UDim.new(0, 10)
    local buttonStroke = Instance.new("UIStroke", button)
    buttonStroke.Color = Color3.fromRGB(255, 255, 255)
    buttonStroke.Thickness = 1.2
    buttonStroke.Transparency = 0.8
    return button
end

local executeButton = createButton("ExecuteButton", "Execute", UDim2.new(0, 15, 0, 245), UDim2.new(0, 120, 0, 45), Color3.fromRGB(40, 80, 220))
local clearButton = createButton("ClearButton", "Clear", UDim2.new(0, 150, 0, 245), UDim2.new(0, 120, 0, 45), Color3.fromRGB(220, 80, 80))
local closeButton = createButton("CloseButton", "Close", UDim2.new(0, 285, 0, 245), UDim2.new(0, 120, 0, 45), Color3.fromRGB(80, 80, 220))
local copyButton = createButton("CopyButton", "Copy", UDim2.new(0, 285, 0, 195), UDim2.new(0, 120, 0, 45), Color3.fromRGB(80, 200, 120))
local minimizeButton = createButton("MinimizeButton", "_", UDim2.new(1, -45, 0, 5), UDim2.new(0, 30, 0, 30), Color3.fromRGB(100, 100, 200))

-- Hover Effect
local function addHoverEffect(button, originalColor)
    button.MouseEnter:Connect(function()
        tweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = originalColor:lerp(Color3.new(1,1,1), 0.1)}):Play()
    end)
    button.MouseLeave:Connect(function()
        tweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = originalColor}):Play()
    end)
end

addHoverEffect(executeButton, Color3.fromRGB(40, 80, 220))
addHoverEffect(clearButton, Color3.fromRGB(220, 80, 80))
addHoverEffect(closeButton, Color3.fromRGB(80, 80, 220))
addHoverEffect(copyButton, Color3.fromRGB(80, 200, 120))
addHoverEffect(minimizeButton, Color3.fromRGB(100, 100, 200))

-- Dragging
local dragging = false
local dragInput, dragStart, startPos

mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

userInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        shadow.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X - 10, startPos.Y.Scale, startPos.Y.Offset + delta.Y - 10)
    end
end)

-- Execute Button
executeButton.MouseButton1Click:Connect(function()
    local code = textBox.Text
    local func, errorMsg = loadstring(code)
    if func then
        task.spawn(func)
        game.StarterGui:SetCore("SendNotification", {
            Title = "System",
            Text = "Your code has been executed successfully!",
            Duration = 5
        })
    else
        game.StarterGui:SetCore("SendNotification", {
            Title = "System",
            Text = "Your code doesn't work or is poorly written.",
            Duration = 5
        })
    end
end)

-- Clear Button
clearButton.MouseButton1Click:Connect(function()
    textBox.Text = ""
end)

-- Copy Button
copyButton.MouseButton1Click:Connect(function()
    setclipboard(textBox.Text)
end)

-- Close Button
closeButton.MouseButton1Click:Connect(function()
    mainFrame:TweenPosition(UDim2.new(0.5, -210, 1, 0), "Out", "Quart", 0.4, true)
    shadow:TweenPosition(UDim2.new(0.5, -220, 1, 10), "Out", "Quart", 0.4, true)
    task.wait(0.45)
    gui:Destroy()
end)

-- Minimize Button
local minimized = false
minimizeButton.MouseButton1Click:Connect(function()
    if not minimized then
        tweenService2:Create(mainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 200, 0, 60)}):Play()
        tweenService2:Create(shadow, TweenInfo.new(0.3), {Size = UDim2.new(0, 220, 0, 80)}):Play()
        scrollingFrame.Visible = false
        executeButton.Visible = false
        clearButton.Visible = false
        closeButton.Visible = false
        copyButton.Visible = false
        minimized = true
    else
        tweenService2:Create(mainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 420, 0, 320)}):Play()
        tweenService2:Create(shadow, TweenInfo.new(0.3), {Size = UDim2.new(0, 440, 0, 340)}):Play()
        task.wait(0.3)
        scrollingFrame.Visible = true
        executeButton.Visible = true
        clearButton.Visible = true
        closeButton.Visible = true
        copyButton.Visible = true
        minimized = false
    end
end)

-- Entry Animation
mainFrame.Position = UDim2.new(0.5, -210, 1, 0)
shadow.Position = UDim2.new(0.5, -220, 1, 10)
mainFrame:TweenPosition(UDim2.new(0.5, -210, 0.5, -160), "Out", "Quart", 0.4, true)
shadow:TweenPosition(UDim2.new(0.5, -220, 0.5, -170), "Out", "Quart", 0.4, true)
