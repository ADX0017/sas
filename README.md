local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local toggle = false
local lastUsed = 0
local cooldown = 10 -- คูลดาวน์ 10 วิ

UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.E then
		local currentTime = tick()
		if currentTime - lastUsed >= cooldown then
			lastUsed = currentTime
			toggle = not toggle

			-- ยิงสกิลดาบ
			local swordArgs = {
				Vector3.new(20.989635467529297, 5.021446228027344, 38.02826690673828)
			}
			local sword = player.Character:FindFirstChild("Basic Sword")
			if sword then
				local attackEvent = sword:FindFirstChild("RemoteEvents"):FindFirstChild("Attack")
				if attackEvent then
					attackEvent:FireServer(unpack(swordArgs))
				end
			end

			-- ใช้ท่า stance
			local stanceArgs
			if toggle then
				stanceArgs = {
					player.Character:WaitForChild("Talents"):WaitForChild("Defensive Stance"),
					Vector3.new(-37.4492301940918, 97.99998474121094, 10.432012557983398)
				}
			else
				stanceArgs = {
					player.Character:WaitForChild("Talents"):WaitForChild("Offensive Stance"),
					Vector3.new(-39.132102966308594, 97.99998474121094, 10.124141693115234)
				}
			end
			ReplicatedStorage:WaitForChild("RemoteEvents"):WaitForChild("Other"):WaitForChild("UseAbility"):FireServer(unpack(stanceArgs))
		end
	end
end)
--end
