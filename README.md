# Tactical-Game
class Unit {
    constructor(name, health, attack, defense, movement) {
        this.name = name;
        this.health = health;
        this.attack = attack;
        this.defense = defense;
        this.movement = movement;
        this.position = { x: 0, y: 0 };
    }

    move(x, y) {
        this.position = { x, y };
        console.log(`${this.name} moves to position (${this.position.x}, ${this.position.y})`);
    }

    attack(target) {
        const damage = Math.max(0, this.attack - target.defense);
        target.health -= damage;
        console.log(`${this.name} attacks ${target.name} for ${damage} damage!`);
    }

    isAlive() {
        return this.health > 0;
    }

    getStatus() {
        return `${this.name} - Health: ${this.health}, Attack: ${this.attack}, Defense: ${this.defense}, Position: (${this.position.x}, ${this.position.y})`;
    }
}

const printStatus = (player, enemy) => {
    console.log("\nGame Status:");
    console.log(player.getStatus());
    console.log(enemy.getStatus());
};

const gameLoop = () => {
    const player = new Unit("Soldier", 100, 30, 10, 5);
    const enemy = new Unit("Enemy", 80, 25, 8, 4);

    const promptAction = () => {
        const action = prompt("Choose action (move/attack): ").toLowerCase();

        if (action === "move") {
            const x = parseInt(prompt("Enter X position (0-5): "));
            const y = parseInt(prompt("Enter Y position (0-5): "));
            player.move(x, y);
        } else if (action === "attack") {
            player.attack(enemy);
        } else {
            console.log("Invalid action! Try again.");
            promptAction();
            return;
        }

        // Enemy's turn
        if (enemy.isAlive()) {
            const enemyAction = Math.random() < 0.5 ? "move" : "attack";
            if (enemyAction === "move") {
                const x = Math.floor(Math.random() * 6);
                const y = Math.floor(Math.random() * 6);
                enemy.move(x, y);
            } else {
                enemy.attack(player);
            }
        }

        if (player.isAlive() && enemy.isAlive()) {
            printStatus(player, enemy);
            promptAction();
        } else {
            if (player.isAlive()) {
                console.log("You defeated the enemy!");
            } else {
                console.log("You were defeated by the enemy.");
            }
        }
    };

    printStatus(player, enemy);
    promptAction();
};

// Start the game
gameLoop();
