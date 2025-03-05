# report
Reporter action for github/gitea workflows

## Usage
### Simple scan with reporting to project and FireClover Vulnerability Managment
```
jobs:
  build:    
    - name: 'Install, build and test application with Docker image'
      run: echo 'See FireClover Docker build action for help'

    - name: Run the FireClover SCA scan action
      uses: fc-actions/scan-sca@v0.1.5
      id: sca
      with:
        source: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.ref_name }}

    - name: Upload vulnerability report
      needs: [steps.sca]
      uses: fc-actions/report@v0.1.11
      with:
        create-repo-issues: 'Critical High'
        instance-url: ${{ vars.FARADAY_URL }}
        password: ${{ secrets.FARADAY_PASSWD }}
        workspace: ${{ env.CUSTOMER_ID || env.STAR_DEPLOYMENT_VANITY_SUBDOMAIN }}
        result-file: ${{ steps.sca.outputs.vulns }}
```
