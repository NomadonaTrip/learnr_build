# B-MAD Method Installation

This directory contains the B-MAD (Breakthrough Method for Agile AI-Driven Development) framework for this project.

## About B-MAD

B-MAD is a revolutionary AI-driven agile framework that automatically adapts from single bug fixes to enterprise-scale systems. It provides:

- **Scale-Adaptive Intelligence**: 3 planning tracks (Quick Flow, BMad Method, Enterprise)
- **Four-Phase Methodology**: Analysis → Planning → Solutioning → Implementation
- **Specialized AI Agents**: 12+ agents including PM, Analyst, Architect, Developer, and more
- **Guided Workflows**: 34+ workflows for different development phases

## Directory Structure

```
.bmad/
├── core/           # Core framework + BMad Master agent
├── bmm/            # BMad Method (12 agents, 34 workflows)
├── _cfg/           # Your customizations (survives updates)
│   └── agents/     # Agent customization files
```

## Key Agents

The BMM module includes these specialized agents:

- **analyst**: Creates Product Requirement Documents (PRD) from project briefs
- **architect**: Designs system architecture based on PRD
- **pm**: Project manager for planning and coordination
- **sm**: Scrum master for agile workflow management
- **dev**: Developer agent for implementation
- **tea**: Test Architect for testing strategy
- **ux-designer**: UX design and specifications
- **tech-writer**: Technical documentation

## Workflows

Workflows are organized by development phase:

1. **Analysis** (`1-analysis/`): Brainstorming, research, product briefs
2. **Planning** (`2-plan-workflows/`): PRD, tech specs, game design docs
3. **Solutioning** (`3-solutioning/`): Architecture, security, DevOps
4. **Implementation** (`4-implementation/`): Story-centric development

## Getting Started

### Three Planning Tracks

1. **Quick Flow Track** - Bug fixes and small features
   - Fast implementation with tech-spec only
   - Best for 2-3 related changes

2. **BMad Method Track** - Products and platforms
   - Full planning (PRD + Architecture)
   - Story-centric implementation

3. **Enterprise Track** - Enterprise requirements
   - Extended planning with security, DevOps, testing
   - Compliance-ready documentation

### Starting a Project

To begin working with B-MAD:

1. Load an agent in your IDE (e.g., `/bmad:bmm:agents:analyst`)
2. Run workflow-init to set up your project path
3. Choose your planning track based on project needs
4. Follow the guided workflows for your chosen phase

## Documentation

For more detailed information:

- [BMM Complete Documentation](./bmm/docs/README.md)
- [Agents Guide](./bmm/docs/agents-guide.md)
- [Quick Start Guide](./bmm/docs/quick-start.md)
- [Workflow Guides](./bmm/docs/)

## License

B-MAD Method is licensed under the MIT License.
BMAD™ and BMAD-METHOD™ are trademarks of BMad Code, LLC.
